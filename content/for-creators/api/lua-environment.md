---
title: Lua Environment
aliases:
  - /api/lua-environment
---

# Lua Environment

Luanti uses Lua 5.1. The environment in which Luanti executes mods depends on four factors:

1. The operating system
2. The Luanti build: [LuaJIT](/for-creators/api/luajit) (default) or PUC Lua 5.1
3. The mod type: Client-side or server-side
4. The mod environment: Secure or insecure

## Platform Independence / Portability

See the [Lua 5.1 Reference Manual](https://www.lua.org/manual/5.1/manual.html) for platform- and OS-environment-dependent Lua features. These include:

- Locale, affecting pattern matching (character classes) and character codes used by `string.char` and `string.byte`
- Large parts of the `os` library, particularly `os.execute` (only available in an insecure environment)
- Some parts of the `io` library like `io.popen` (only available in an insecure environment) or handling of binary files
- `require` and `package.loadlib` (only available in an insecure environment anyways)

## Global Strictness

Variables in Lua are global by default (both assignment and access). This often leads to mistaken use of global variables, with the two perhaps most common issues being:

1. Misspelling a local variable and accessing a global variable instead (which will usually be `nil`)
2. Forgetting `local` when assigning to a variable, (over)writing a global variable, leading to "global pollution"

Luanti's built-in strictness works using a metatable on the global table and will log warnings for both cases. Luanti defines a global declaration as a _global assignment in a main chunk_ - setting globals in any other function will not be considered a declaration and will trigger a warning:

```lua
-- Declarations according to Luanti's definition:
global_var = 42
for k, v in pairs{a = 1, b = 2} do _G[k] = v end
-- Assignment to undeclared global:
local function set_global()
    another_global_var = 42
end
set_global()
```

1. Reading an undeclared global variable will trigger an "undeclared global variable access" warning
2. Setting an undeclared global variable after load time will trigger an "assignment to undeclared global" warning

Warnings are identified by their location as returned by `debug.getinfo` (`short_src` and `currentline`) and won't be logged twice.

{{< notice warning >}}
Accessing undeclared global variables will be an order of magnitude slower than accessing declared globals due to the executed strictness checking code.
{{< /notice >}}

{{< notice tip >}}
These warnings are only triggered at run time as the global variable access or assignment occurs. It is recommended to use a linter like [`luacheck`](https://github.com/mpeterv/luacheck) to detect mistaken global variable usage statically at the time of development.
{{< /notice >}}

### Checking for global existence

For mod compatibility, the existence of global variables must be checked. A simple `if name then ... end` check might trigger an "undeclared global variable access" warning if the variable doesn't exist (is `nil`) and has not been declared either (usually when an optional dependency isn't present).

As Luanti implements global strictness over a metatable, `rawget(_G, name)` can be used in place of just `name` to access possibly `nil` globals without triggering a warning. Similarly, `rawset(_G, name, value)` may be used to set globals at run time.

#### `core.global_exists(name)`

Returns `true` if a global variable with the given `name` exists (is not `nil`), `false` otherwise. An error is thrown if `name` is not a string.

{{< notice note >}}
This wraps `rawget(_G, name)` in the end but might be considered more readable as it makes the intention clear.
{{< /notice >}}

## Standard Library Extensions

{{< notice note >}}
It is considered bad practice to extend the standard library yourself, as this may collide with other mods doing the same as well as future engine changes including Lua version upgrades. Put your extensions into distinct API tables instead of modifying Lua's builtin libraries.
{{< /notice >}}

### `math`

#### `math.hypot(x, y)`

Finds the length of the hypotenuse `z` according to the Pythagorean Theorem: stem:[z^2 = x^2 + y^2]. Shorthand for `math.sqrt(x*x + y*y)`.

#### `math.sign(x, [tolerance])`

`tolerance` defaults to `0` if falsy. Returns `-1` if the value is smaller than the `tolerance`, `1` if it is larger. Returns `0` if `x` is within the closed tolerance interval `[-tolerance, tolerance]`. Also returns `0` if `x` is `nan`.

#### `math.factorial(x)`

`x` must be a non-negative integer; otherwise, the function will error with `"factorial expects a non-negative integer"`. If `x` is at least `171`, `+inf` is returned.

As Python has built-in big integer support (and uses 64-bit `float`), it can be used to easily determine for which `x` this implementation becomes imprecise due to float precision limitations:

```python
def factorial(x):
	return x if x ## 1 else x * factorial(x-1)
for x in range(1, 171):
	if factorial(float(x)) != factorial(x):
		print(x)
		break
```

This will print `23`. This means that only for `x` values ranging from `1` to `22`, both inclusive, `factorial(x)` will be fully accurate.

#### `math.round(x)`

Rounds `x` towards the nearest integer value. Edge cases:

- Ties: If `x` is exactly the same distance from two integer values (`x = k + 0.5`) with `k` being an integer, it is rounded "away from zero", to the value with the higher absolute value:
  - If `x > 0`, `x` will be rounded to the larger value;
  - If `x < 0`, `x` will be rounded to the smaller value.
- Precision: Numbers very close to ties (plus/minus `0.49999999999999994`) are incorrectly handled like ties. See [this StackOverflow answer on rounding in Lua](https://stackoverflow.com/a/58411671/7185318).
- Special float values: `nan` and `inf` are preserved, as well as their sign.

### `string`

These functions can be used over the string metatable as well, using `self:func(...)` if `self` is a string.

Note. `"...":func(...)` is a syntax error in Lua. Wrap strings in brackets `(...)` if you want to index them: `("..."):func(...)`.

#### `string.trim(self)`

Will return a string with consecutive spacing characters (`%s` pattern character class) at the start and the end of the string removed. Usually the following characters are considered spacing:

- Horizontal tab: `'\t'` or `'\9'`
- Newline: `'\n'` or `'\10'`
- Vertical tab: `'\v'` or `'\11'`
- Form feed: `'\f'` or `'\12'`
- Carriage return: `'\r'` or `'\13'`
- Space: `' '` or `'\32'`

As determined using the below Lua script, which outputs the decimal character codes:

```lua
for i = 0, 255 do
	if string.char(i):match"%s" then
		print(i)
	end
end
```

{{< notice warning >}}
Platform-independence is not guaranteed: "The definitions of letter, space, and other character groups depend on the current locale." - [Lua 5.1 Reference Manual, section 5.4.1](https://www.lua.org/manual/5.1/manual.html#5.4.1)
{{< /notice >}}

#### `string.split(str, [delim], [include_empty], [max_splits], [sep_is_pattern])`

- `str`: The string to split.
- `delim`: Delimiter/separator. Defaults to `","` if falsy.
- `include_empty`: If truthy, empty strings (`""`) are included in the returned list.
- `max_splits`: Maximum amount of splits to be done. Splits are done in left-to-right (string start to end) order. The resulting list can have up to `max_splits + 1` entries. The last element in the list may contain the delimiter. Unlimited splits if falsy, negative, `nan` or `+inf`.
- `sep_is_pattern`: If truthy, `delim` is used as a pattern.

Returns a list containing the delimited parts without the delimiters.

### `table`

#### `table.indexof(list, val)`

Linear search for `val` in the `list`. Returns the first index where the value equals `val`. Returns `-1` if the value is not found.

#### `table.copy(t, [seen])`

Deep-copies the table `t` and all it's subtables - both keys and values. Non-table types are not copied, even if they are reference types (userdata, functions and threads). The reference structure will be fully preserved: A single table, even if referenced multiple times, will only be copied a single time; subsequent references in the copy will just reference the same copied table.

The `seen` table is a lookup for already copied tables, which are used as keys. The value is the copy. By providing `[table] = table` entries for certain tables, you can prevent them from being copied.

Example: Preservation of referential structure means the `assert`ion in the following code will work:

```lua
a = {}; b = {a, a}; c = table.copy(b); assert(c[1] ## c[2])
```

A different deep cloning implementation might clone `a` twice, leading to `c[1] ~= c[2]`.

#### `table.insert_all(t, other)`

Adds all the list entries of `other` to `t` (list part concatenation).

#### `table.key_value_swap(t)`

Returns a new table with the keys of `t` as values and the corresponding values as keys. If a value occurs multiple times in `t`, any of the keys might be the value in the resulting table.

#### `table.shuffle(t, [from], [to], [random])`

Performs a Fisher-Yates shuffling on the specified range of the list part of `t`.

- `from`: Inclusive starting index of the range to be shuffled. Defaults to the first item of the list part if falsy.
- `to`: Inclusive end index of the range to be shuffled. Defaults to the last item of the list part if falsy.
- `random`: A `function(from, to)` that returns a random integer in the specified range, with both `from` and `to` inclusive. Defaults to `math.random` if falsy.

Returns nothing.

## LuaJIT extensions

Luanti builds compiled with LuaJIT (`ENABLE_LUAJIT=1`) provide the [LuaJIT extensions](https://luajit.org/extensions.html). These include syntactical Lua 5.2 language features like `goto`, which will lead to a syntax error on PUC Lua 5.1. Hex escapes will be converted into the raw characters by PUC Lua 5.1 (Example: `"\xFF"` which is the same as `"\255"` on LuaJIT will be `"xFF"` on PUC Lua 5.1).

## Common extensions

[LuaJIT's `bit` library](https://bitop.luajit.org/) is made available for both PUC Lua and LuaJIT builds. It must not be required, as this will lead to a crash in a secure environment as documented below; in an insecure environment, it is simply unneeded.

## Secure environment whitelists

In the secure environment, the following builtin Lua(JIT) libraries and library functions are whitelisted:

- `_VERSION`
- Garbage collection: `collectgarbage`
- Cooperative multithreading: `coroutine`
- Error handling:
  - `assert`
  - `error`
  - `pcall`
  - `xpcall`
- Function environments:
  - `setfenv`
  - `getfenv`
- `math`
- `string`
- Tables:
  - `table`
  - Iteration:
    - `next`
    - `pairs`
    - `ipairs`
- Metatables:
  - `setmetatable`
  - `getmetatable` (SSM-only)
  - Raw methods:
    - `rawset`
    - `rawget`
    - `rawequals`
- Varargs:
  - `select`
  - `unpack`
- Conversion:
  - `tostring`
  - `tonumber`
- `type`
- Output: `print`

Some library tables are restricted by whitelists as well:

- `io` (SSM-only)
  - `read`
  - `write`
  - `flush`
  - `close`
  - `type`
- `os`: Mostly time-related functions
  - `clock`
  - `date`
  - `difftime`
  - `time`
  - SSM-only:
    - `getenv`
    - `setlocale`: Severely restricted, can only be used to get locale
- `debug`:
  - `gethook`
  - `traceback`
  - SSM-only:
    - `getinfo`
    - `getmetatable`
    - `setmetatable`
    - `upvalueid`
    - `sethook`
    - `debug`
- `package` (SSM-only):
  - `config`
  - `cpath`
  - `path`
  - `searchpath`
- `jit` (LuaJIT-only):
  - `arch`
  - `flush`
  - `off`
  - `on`
  - `opt`
  - `os`
  - `status`
  - `version`
  - `version_num`

Everything file-related is replaced by a secure variant:

- Loading Lua code: Errors with `"Bytecode prohibited when mod security is enabled."` if the sources are bytecode (strings starting with `'\27'`). For CSM, the file-related functions operate on virtual paths and only have access to CSM files.
  - `dofile`
  - `load`
  - `loadfile`
  - `loadstring`
  - `require`: Disabled, errors with `"require() is disabled when mod security is on."`.

The following functions, which are _not available to CSM / SSM-only_, allow read-only access to all mod directories and write access to the current loading mod's directory only while it's loading (a handle with write access to a file within the mod directory can however be stored and used at a later time) and read & write access to the world directory excepting the `worldmods` and `game` subfolders:

- `io`:
  - `open`
  - `input`
  - `output`
  - `lines`
- `os`:
  - `remove`
  - `rename`

Builtin can read and write anywhere during it's load time.

See the [Lua 5.1 Reference Manual](https://www.lua.org/manual/5.1/manual.html) for documentation of the Lua standard library.

If mod security is disabled, server-side mods run in an insecure environment, which contains all libraries and library functions, without any restrictions. The same restrictions apply to trusted server-side mods, which can however request an insecure environment in table form using `core.request_insecure_environment` which will contain shallow copies of library tables and no global restrictions.
