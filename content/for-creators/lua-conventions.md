---
title: Lua Conventions
aliases:
  - /lua-conventions
  - /content-dev/lua-conventions
---

Lua is a simple, minimalistic, elegant, efficient, embeddable, expressive scripting language.
A good grasp of Lua is required to effectively develop Luanti mods and games.

Lua is a very flexible - perhaps for the taste of many too flexible - language.

> Lua gives you the power, you build the mechanisms
> \- ["Programming in Lua", first edition](https://www.lua.org/pil)

This document describes some mechanisms and conventions used in Luanti.

## Resources

This does not teach you programming in general or the basics of Lua programming specifically.
Refer to the following resources instead:

- **Strongly recommended: [Programming in Lua (PIL)](https://www.lua.org/pil)**
  - The online version was written for 5.0, but is for the most part applicable to 5.1.
- The [`lua-users.org` tutorial](http://lua-users.org/wiki/LuaTutorial)
- Good to have at hand: [The Lua 5.1 reference manual](https://www.lua.org/manual/5.1/)
- [Lua 5.1 short reference ("cheat sheet")](https://web.archive.org/web/20230308221513/http://thomaslauer.com/download/luarefv51.pdf)

## Integers

Lua 5.1 only has a single number type: [64-bit IEEE754 floats](https://en.wikipedia.org/wiki/IEEE_754).

Unless stated otherwise, Luanti typically expects _finite_ numbers:
No positive or negative infinities (`math.huge`, `-math.huge`), and no "not-a-number" (`0/0`) values.

These can represent many whole numbers exactly.
Hence a number with a fractional part of zero (`math.floor(number) == number`)
is called an _integer_.

All integers from `-2^53` to `2^53` can be represented exactly.
These are called the _safe_ integers. You should stick to these.
Larger or smaller integers are no longer contiguous (for example `2^53 + 1` can not be represented exactly).

Be careful: Often Luanti will internally convert Lua numbers into 32-bit
(or even smaller) signed integers in the C++ implementation.
In fact, even PUC Lua 5.1 itself does this, e.g. for `math.random`
(and even worse, it then does integer arithmetic on them,
so you have to ensure not only that your numbers are in-range,
but also that the difference between them is.)

Hence it is typically a good idea to stay well above the `-2^31` and below the `2^31 - 1` limits.

You should not pass non-integral values to functions that expect integers,
including built-in Lua functions like `math.random`.
How these values are rounded is not specified.
You should instead use `math.floor`, `math.ceil` or `math.round` yourself.

## Strings

Lua strings are byte strings. This means you are, in principle, free to use any encoding,
but the established standard used by Luanti is [UTF-8](https://en.wikipedia.org/wiki/UTF-8).
Whenever you are encoding or decoding _Unicode text_, you should expect UTF-8 unless stated otherwise.

Note that functions such as `core.compress` obviously do not operate on Unicode text
but instead treat strings as _binary data_.

Do not confuse these two usages of Lua strings.

## Tables

Lua's primary "all-in-one" data structuring mechanism is the table.

Tables map _keys_ to _values_. A key-value pair is sometimes also called an _entry_.

The table `{a = 1, ["b"] = 2, [3] = true}` has the keys `"a"`, `"b"` and `3`
and the values `1`, `2` and `true`. The pair `3`, `true` is an entry:
The key `3` is mapped to the value `true`.

A _list_ (sometimes also called a _sequence_)
is a table with consecutive integer keys starting at `1`.
For example `{1, 2, 3}` and `{[3] = 3, [1] = 1, [2] = 2}` are lists.
Note that unless otherwise stated, a list can not have "holes":
`nil` values followed by non-`nil` values.
For example `{nil, 2}` and `{1, [3] = 3}` are not valid lists.

A _set_ is a table where the keys are considered to be the elements of the set.
The values should be `true` (or at the very least not `false`) per convention.
For example `{spam = true, ham = true}` is a set.
`{spam = true, ham = false}` is not a set.
`{"bread", "bacon", "eggs"}` is a set containing the elements `1`, `2` and `3`
(recall that the values are insignificant in a set).
A more suggestive way to write it would be `{[1] = true, [2] = true, [3] = true}`.

A global table consisting of names as keys and API functions or tables is called a _namespace_ (or API table).
For example `core` is a namespace.
It is strongly recommended that you namespace the APIs
you want your mods to expose (and keep everything else `local`).

For example:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
mymath = {}

function mymath.clamp(x, min, max)
    return math.max(math.min(x, max), min)
end
```

It is important to be precise about the structure of tables.
One possible mistake for example is to supply a list of textures as
`{name = "mymod_mytexture.png", backface_culling = false}`
rather than `{ { name = "mymod_mytexture.png", backface_culling = false } }`.
{{% comment %}} cspell:enable {{% /comment %}}

The former will probably be treated like an empty list,
while the latter will correctly be treated as a list
containing a single texture.

## Object-oriented programming

Lua does not bring classes out of the box. Instead, Lua uses metatables,
which allows implementing [prototype-based object-oriented programming](https://en.wikipedia.org/wiki/Prototype-based_programming).
Luanti leverages this to implement its own form(s) of _classes_.

Usually Luanti only uses the `__index` metatable field to set a prototype.
A notable exception are vectors, which have overloaded arithmetic operators
(see [Spatial Vectors](https://api.luanti.org/spatial-vectors/#spatial-vectors) for details).

A _constructor_ is a function that returns an _instance_ of a class.
Examples are `VoxelManip(...)`, `VoxelArea:new(...)` or `vector.new(...)`.

As you can see, constructors are inconsistent due to historical reasons.
Some constructors have to be called as `Class:new(...)`, others use `Class.new(...)`,
and yet some others use `Class(...)`.

Instances may be either tables or opaque userdata objects.
If an instance is a table, the fields unique to that table
(that is, all the fields that are not provided by the metatable)
are called the _instance variables_ or _instance fields_.

A _method_ is a function operating on instances of a class.
Methods are usually called as `instance:method(...)`,
which is nothing but syntactic sugar for `instance.method(instance, ...)`.

Similarly to methods, there may be _class variables_ shared among all instances.
These can typically be accessed via `class.variable` or `instance.variable`.

The way metatables work, `instance.field` will resolve to `class.field`
via the `__index` field / metamethod.

This means that `instance.class_variable`
will (usually) resolve to the very same value for different instances.

If `instance.class_variable` is a reference type, this may be a source of so-called "aliasing" bugs:
Unknowingly mutating a class variable (which may look like an instance variable)
mutates it for all instances.

An example are Lua entities.
Each Lua entity registration registers its own class to go along with it.
Lua entities are all instances of their respective classes.
(Not to be confused with "objects", which are instances of the C++ ObjectRef "class" and userdata.
Lua entities are in a 1:1-correspondence with objects:
The associated object can be obtained as `entity.object`,
and the associated entity for an object can be obtained as `object:get_luaentity()`.)

If you do `entity.initial_properties.textures = {"foo.png"}`,
you're changing the initial properties class variable (which is a table)
for all entities of the same type.
