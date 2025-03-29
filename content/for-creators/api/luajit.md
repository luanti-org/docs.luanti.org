---
title: LuaJIT
aliases:
  - /luajit
  - /LuaJIT_differences
  - /luajit-differences
  - /content-dev/luajit-differences
---

# LuaJIT

LuaJIT is a just-in-time compiler for Lua, which implements the language as it was in the reference Lua 5.1 implementation (referred to as PUC Lua to distinguish it) albeit with some additions and differences. LuaJIT typically executes code much faster than PUC Lua, which is why its usage is strongly recommended.

More often than not, unless you have compiled Luanti from source without LuaJIT, Luanti will use LuaJIT as its Lua runtime. The official Windows, Android and most packaged Linux builds should be compiled with LuaJIT enabled. To confirm what Lua runtime Luanti is built with you can open a terminal and run `./luanti --version`.

There are some reasons one would want to use PUC Lua over LuaJIT such as for stability or if someone is on an obscure architecture that doesn't have a LuaJIT compiler available yet. As such, you should always support both implementations in your mods and be aware of some of the pitfalls that can cause crashes with a mod developed on LuaJIT being run with PUC Lua.

If you are using LuaJIT the result would look something like this:

```shell
$ ./luanti --version
[...]
Using LuaJIT 2.1.1702233742
```

If you are using PUC Lua the result would look something like this:

```shell
$ ./luanti --version
[...]
Using Lua 5.1.5
```

When building from source, Luanti will always use LuaJIT if it is found on the system. This usually means having the development package for it installed. If LuaJIT is not found or if `-DENABLE_LUAJIT=FALSE` is explicitly passed then Luanti will fall back to the vendored PUC Lua 5.1.

See also [Lua environment](/for-creators/api/lua-environment).

## LuaJIT differences

### Iteration order of tables

The Lua reference manual **does not guarantee an iteration order of tables**:
`pairs` (and `next`) are allowed to visit the table entries in **any order**.
You should **never** rely on a table being iterated in a certain order,
regardless of the Lua implementation you are using.

The iteration order of LuaJIT and PUC Lua will usually be different because they
use different implementations internally.
In particular, LuaJIT for security reasons uses a random seed in its hashing,
causing iteration orders to differ across different runs.

### Goto

LuaJIT implements the `goto` statement along with labels that can be jumped to as an extension to the language. This is not available in PUC Lua, so you shouldn't use `goto` in your mods.

### Hexadecimal escape codes

LuaJIT supports hexadecimal escape codes in strings. For example the string `"\x41"` is equivalent to `"A"` on LuaJIT.
PUC Lua however simply ignores the backslash; it will not throw an error: `"\x41"` is suddenly equivalent to `"x42"`,
which is certainly not what you want.

Instead, you should use PUC Lua's 3-digit [^1] decimal escapes: `"\06542"` is `"A42"`.

### xpcall()

LuaJIT allows you to pass extra arguments to the function `f` to be called in a protected context.
For example `xpcall(print, err_func, "hello world")` would pass `"hello world"` to `print`,
whereas on PUC Lua, the `"hello world"` would just be ignored.

### math.log()

LuaJIT lets you optionally specify a base for the logarithm, PUC Lua only knows the natural logarithm
and ignores extraneous arguments.

Hence `math.log(x, base)` should be written as `math.log(x) / math.log(base)` to be portable.

### math.random()

#### Use of a different generator

LuaJIT brings its own pseudo-random number generator (PRNG) whereas PUC Lua just uses a PRNG provided by your system.
This means that given the same seed via `math.randomseed`, they will almost certainly produce different sequences.

In order for mapgen code to be portable (to yield consistent, reproducible results on PUC Lua and LuaJIT),
you **must not use `math.random()` in mapgen code**.

#### Argument order independence

LuaJIT is less strict about the order of arguments passed to `math.random`. If you pass a min value that is larger than the max value, it will switch these around. For instance `math.random(3,1)` will swap around the ranges and generate the same random values `math.random(1,3)` would generate.

In PUC Lua doing this will throw an error "bad argument #2 to 'random' (interval is empty)", so make sure you clamp or otherwise make sure that the min value is lower or equal to the max value, swapping them yourself if necessary.

### More...

An exhaustive list of the extensions to the Lua language that LuaJIT provides can be found on [LuaJIT.org's extensions page](https://luajit.org/extensions.html).

### For older versions of Luanti

Since 5.5.0 the [bitop](https://bitop.luajit.org/index.html) library is included when building Luanti with PUC Lua so if your mod targets Luanti 5.5+ it is safe to use the bitop library. This has always been available on LuaJIT, but you cannot rely on it being available on Luanti 5.4 and earlier.

[^1]: Leading zeros can be omitted, but as this example demonstrates it may be necessary to keep them in case the escape sequence is followed by literal digits.
