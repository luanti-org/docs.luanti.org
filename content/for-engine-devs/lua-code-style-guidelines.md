---
title: Lua code style guidelines
aliases:
  - /Lua_code_style_guidelines
  - /lua-code-style-guidelines
  - /engine-dev-process/lua-code-style-guidelines
---

This is largely inspired by [the Python style guide](https://www.python.org/dev/peps/pep-0008) except for some indentation and
language differences. When in doubt, consult that guide (if applicable).

Note that these are only _guidelines_ for more readable code. In some (rare) cases they may result in _less_ readable code. Use your best judgement.

## Comments

- Incorrect or outdated comments are worse than no comments.
- Avoid inline comments, unless they're very short.
- Write comments to clarify anything that may be confusing. Don't write comments that describe things that are obvious from the code.

  Good:

  ```lua
  width = width - 2  -- Adjust for 1px border on each side
  ```

  Bad:

  ```lua
  width = width - 2  -- Decrement width by two
  ```

- Comments should follow English grammar rules, this means **starting with a capital letter**, using commas and apostrophes where appropriate, and **ending with a period**. The period may be omitted in single-sentence comments. If the first word of a comment is an identifier, its casing should not be changed. **Check the spelling.**

  {{% comment %}} cspell:disable {{% /comment %}}

  ```lua
  -- This is a properly formatted comment.
  -- This isnt.              (missing apostrophe)
  -- neither is this.        (lowercase first letter)
  ```

  {{% comment %}} cspell:enable {{% /comment %}}

- Comments should have a space between the comment characters and the first word.

  ```lua
  --This is wrong.
  -- This is right.
  ```

- Inline comments should be separated from the code by two spaces.

  ```lua
  foo()  -- A proper comment.
  ```

- If you write comments for a documentation generation tool, write the comments in LuaDoc format.
- Short multi-line comments should use the regular single-line comment style.
- Long multi-line comments should use Lua's multi-line comment format with no leading characters except a `--` before the closer.

  ```lua
  --[[
  Very long
  multi-line comment.
  --]]
  ```

## Line wrapping, spaces, and indentation

- Indentation is done with one tab per indentation level.
- **Lines are wrapped at 80 characters** where possible, with a hard limit of 90. If you need more you're probably doing something wrong.

### Continuation lines

- Conditional expressions have continuation lines indented by two tabs.

  ```lua
  if long_function_call(with, many, arguments) and
  		another_function_call() then
  	do_something()
  end
  ```

- Function arguments are indented by two tabs if multiple arguments are in a line, same for definition continuations.

  ```lua
  foo(bar, biz, "This is a long string..."
  		baz, qux, "Lua")

  function foo(a, b, c, d,
  		e, f, g, h)
  	[…]
  end
  ```

- If the function arguments contain a table, it's indented by one tab and if the arguments get own lines, it's indented like a table.

  {{% comment %}} cspell:disable {{% /comment %}}

  ```lua
  register_node("name", {
  	"This is a long string...",
  	0.3,
  })

  list = filterlist.create(
  	preparemodlist,
  	comparemod,
  	function()
  		return "no comma at the end"
  	end
  )
  ```

  {{% comment %}} cspell:enable {{% /comment %}}

- When strings don't fit into the line, you should add the string (changes) to the next line(s) indented by one tab.

  {{% comment %}} cspell:disable {{% /comment %}}

  ```lua
  longvarname = longvarname ..
  	"Thanks for reading this example!"

  local retval =
  	"size[11.5,7.5,true]" ..
  	"label[0.5,0;" .. fgettext("World:") .. "]" ..
  	"label[1.75,0;" .. data.worldspec.name .. "]"
  ```

  {{% comment %}} cspell:enable {{% /comment %}}

- When breaking around a binary operator you should break after the operator.

  ```lua
  if a or b or c or d or
  		e or f then
  	foo()
  end
  ```

### Empty lines

- Use a single empty line to separate sections of long functions.
- Use two empty lines to separate top-level functions and large tables.
- Do not use a empty line after conditional, looping, or function opening statements.

  Good:

  ```lua
  function foo()
  	if x then
  		bar()
  	end
  end
  ```

  Bad:

  ```lua
  function foo()

  	if x then

  		bar()
  	end
  end
  ```

- Don't leave white-space at the end of lines.

### Spaces

- Spaces are not used around parentheses, brackets, or curly braces.

  Good:

  ```lua
  foo({baz=true})
  bar[1] = "Hello world"
  ```

  Bad:

  ```lua
  foo ( { baz=true } )
  bar [ 1 ]
  ```

- Spaces are used after, but not before, commas and semicolons.
- Spaces are used around binary operators with following exceptions:

  - There must not be spaces around the member access operator (`.`).
  - Spaces around the concatenation operator (`..`) are optional.
  - In short one-line table definitions the spaces around the equals sign can be omitted.

  Good:

  ```lua
  local num = 2 * (3 / 4) + 1
  foo({bar=true})
  foo({bar = true})
  local def = {
  	foo = true,
  	bar = false,
  }
  ```

  Bad:

  ```lua
  local num=2*(3/4)
  local def={
  	foo=true,
  	bar=false,
  }
  ```

- Use spaces to align related things, but don't go overboard:

  Good:

  ```lua
  local node_up   = core.get_node(pos_up)
  local node_down = core.get_node(pos_down)
  -- Too long relative to the other lines, don't align with it
  local node_up_1_east_2_north_3 = core.get_node(pos_up_1_east_2_north_3)
  ```

  Bad:

  ```lua
  local x                       = true
  local very_long_variable_name = false

  local foobar    = true
  local unrelated = {}
  ```

## Numbers

- Do not add a trailing `.0` or `.` to integers.
  (All numbers are doubles under the hood anyways,
  and doubles represent integers from `-2^53` to `2^53` exactly.)
- Do not start numbers with just the decimal point. Write `0.5` instead of `.5`.
- You may use exponential notation for numbers with many trailing or leading zeroes:
  `1e3`, `1e-3` instead of `1000`, `0.001`.
- You may use hexadecimal notation when bit representation matters
  (such as for ARGB8 hex values).
- Prefer `^` over `math.pow`.
- Prefer `math.sqrt(x)` over `x ^ 0.5`. [^sqrt]
- Do not rely on string-to-number coercion done by arithmetic operations or `math` functions.
  If you're doing `x + 1` where `x` is a string, write `tonumber(x) + 1` instead to explicitly do the conversion.

[^sqrt]:
    The two differ very slightly in behavior, leading to `math.sqrt` being an order of magnitude faster on LuaJIT.
    See [this LuaJIT issue](https://github.com/LuaJIT/LuaJIT/issues/684) for details.

## Strings

- Use double-quoted strings (`"..."`) by default.
  You may use single-quoted strings (`'...'`) to save some escapes.
  You may use long strings (`[[...]]`) if you need multiline strings.
- Use "method"-style to call `string.*` functions: `s:find("luanti")` instead of `string.find(s, "luanti")`.
  (Exceptions are made for `string.char`, which does not take a string as its first argument,
  and for `string.format`, where the first argument is typically a string literal.)
- Use `s == ""` to check whether a string is empty.
- Use `#s` (instead of `s:len()`) to get the length of a string.
- Prefer pattern matching over manual string manipulation if it is simpler.
- You may rely on `"number: " .. num`, as well as `table.concat({"number: ", num})`,
  converting a number `num` to a string, but explicit format strings or explicit application of `tostring` are often preferable.
- You must not rely on number-to-string coercion done by `string` functions except for backward compatibility reasons.
- When building long strings (for example formspecs), prefer `table.concat` over string concatenation:

  Bad:

  ```lua
  local s = ""
  for i = 1, 1000 do
  	s = s .. i
  end
  local s2 = a .. b .. c .. d ..
  	"hello" .. e .. "world" ..
  	"foo" .. bar .. "quux"
  ```

  Good:

  ```lua
  local t = {}
  for i = 1, 1000 do
  	t[i] = i
  end
  local s = table.concat(t)
  local s2 = table.concat({
  	a, b, c, d,
  	"hello", e, "world",
  	"foo", bar, "quux",
  })
  ```

## Tables

- **Small tables may be placed on one line.**
  Large tables have one entry per line, with the opening and closing braces on lines without items, and with a comma after the last item.

  Good:

  ```lua
  local foo = {bar=true}
  foo = {
  	bar = 0,
  	biz = 1,
  	baz = 2,
  }
  ```

  Bad:

  ```lua
  foo = {bar = 0,
  	biz = 1,
  	baz = 2}
  foo = {
  	bar = 0, biz = 1,
  	baz = 2
  }
  ```

- In list-style tables where each element is short multiple elements may be placed on each line.

  ```lua
  local first_eight_letters = {
  	"a", "b", "c", "d",
  	"e", "f", "g", "h",
  }
  ```

- Do not mix list-style tables and "dict"-style tables.
- Use `ipairs` to iterate list-style tables, use `pairs` to iterate "dict"-style tables.
- Avoid list-like tables with "holes" (for example `{1, nil, 3}`).
- Use `table.insert` to append items to list-style tables and `table.remove` to remove items from list-style tables.
- Use syntactic sugar. Write `{name = value}` instead of `{["name"] = value}`. Write `t.name` instead of `t["name"]`.
- Do not use semicolons to separate table entries.

## Functions

- Function definition:

  - Always use `function name(...)` instead of `name = function(...)`.
  - Use `local function name(...)` instead of `local name = function(...)`,
    irrespective of whether the function is recursive or not.
  - Use `function class:method(...)` syntactic sugar for "instance method" definitions.
  - You can use `{func = function(...) ... end}` inside tables, but you may consider
    using a variable for the table instead, especially for "namespaces":

    ```lua
    local my = {}
    function my.func(...) ... end
    ```

- Function calls:
  - Use `obj:method(...)` syntactic sugar to call "instance methods".
  - Do not use `f"..."`, `f'...'`, `f[[...]]` or `f{...}` syntactic sugar.
- Prefer returning nothing (default when a function has no return statement) over returning `nil`.

## Naming

{{% comment %}} cspell:disable {{% /comment %}}

- **Functions and variables should be named in `lowercase_underscore_style`**, with the exception of constructor-like functions such as `PseudoRandom()`, which should use UpperCamelCase.
- Don't invent compound words. Common words like `filename` are okay, but mashes like `getbox` and `collisiondetection` aren't.
- Avoid leading and/or trailing underscores. They're ugly and can be hard to see.
  {{% comment %}} cspell:enable {{% /comment %}}

## Miscellaneous

- **Do not use semicolons**. You almost never need to use a semicolon to separate two statements in Lua.
- Don't put multiple statements on the same line.
- You can put conditionals / loops with small conditions and bodies on one line. This is discouraged for all but the smallest ones though.

  Good:

  ```lua
  local f, err = io.open(filename, "r")
  if not f then return err end

  if     foo then return foo
  elseif bar then return bar
  elseif qux then return qux
  end
  ```

  Bad:

  ```lua
  if not f and use_error then error(err) elseif not f then return err end
  ```

- Use `do`-`end` blocks to limit the scope of variables, for example to make
  a `local` variable "private" to a small set of functions:

  ```lua
  local increment_counter
  do
  	local counter = 0
  	function increment_counter()
  		counter = counter + 1
  		print("Counter is now", counter)
  	end
  end
  ```

- Don't compare values explicitly to `true`, `false`, or `nil`, unless it's really needed.

  Good:

  ```lua
  local f, err = io.open(filename, "r")
  if not f then return err end

  local t = {"a", true, false}
  for i = 1, 5 do
  	-- Needs an explicit nil check to avoid triggering
  	-- on false, which is a valid value.
  	if t[i] == nil then
  		t[i] = "Default"
  	end
  end
  ```

  Bad:

  ```lua
  if f == nil then return err end
  ```

- Don't use unnecessary parentheses unless they improve readability a lot.

  ```lua
  if y then bar() end -- Good
  if (not x) then foo() end -- Bad
  ```

- **Avoid globals like the plague.** The only globals that you should create are namespace tables - and even those might eventually be phased out.
- Don't let functions get too large. Maximum length depends on complexity; simple functions can be longer than complex functions.
- Use `not not` to coerce values to their truthiness.
- Iterate varargs using `select`. Store varargs along with their length in tables.
