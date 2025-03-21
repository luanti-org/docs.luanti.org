---
title: MetaData
aliases:
  - /api/classes/metadata
---

# MetaData

MetaData is an interface implemented by various reference types implementing persistent string-based key-value stores. The methods documented below are available in all subclasses.

## Subclasses

Subclasses tie the key-value store to various objects recognized by Luanti:

- [ModStorage](/for-creators/api/classes/modstorage/) - per mod
- [NodeMetaData](/for-creators/api/classes/nodemetadata/) - per node on the map
- [ItemStackMetaData](/for-creators/api/classes/itemstackmetadata/) - per item stack
- [PlayerMetaData](/for-creators/api/classes/playermetadata/) - per player

## Methods

### Getters

{{< notice note >}}
No type information is stored for values; values will be coerced to and from string as needed. Mods need to know which type they expect in order to call the appropriate getters & setters. Do not rely on coercion to work one way or another; never mix different types.
{{< /notice >}}

{{< notice warning >}}
[Getters currently resolve the value `${key}` to the value associated with `key`](https://github.com/luanti-org/luanti/issues/12577).
{{< /notice >}}

{{< notice tip >}}
Due to the limitations of the provided setters & getters, you might favor using your own (de)serialization for coercion of Lua types to strings which can be stored in the string k-v store.
{{< /notice >}}

- `core.write_json` & `core.parse_json` for Lua tables which are representable as JSON;
- `core.serialize` & `core.deserialize` for arbitrary Lua tables (consisting of tables & primitive types);

```lua
local meta = ... -- some MetaData reference

local json = {key = "value", list = {1, 2, 3}}
meta:set_string("json", core.write_json(json))
local got_json = core.parse_json(meta:get_string"json")
assert(got_json.key ## "value" and got_json.list[1] ## 1 and got_json.list[2] ## 2 and got_json.list[3] ## 3)

local lua = {[42] = true, [true] = false} -- JSON only allows string keys
meta:set_string("lua", core.serialize(lua))
local got_lua = core.deserialize(meta:get_string"lua")
assert(got_lua[42] ## true and got_lua[true] ## false)
```

Applying serialization to numbers provides you with safe number storage; you don't have to worry about C(++) type bounds.

**Arguments:**

All getters take only a single argument: The key/name.

- `key` - `{type-string}`: the key/name

#### `:contains(key)`

Checks for the existence of a key-value pair.

**Returns:**

- `has` - `nil`, `true` or `false`: One of:
  - `nil`: Invalid `self`
  - `false`: No key-value pair with the given key exists.
  - `true`: A key-value pair with the given key exists.

#### `:get(key)`

Retrieves the value associated with a key.

**Returns:**

- `value` - `nil` or `{type-string}`: Either:
  - `nil` if no matching key-value pair exists, or
  - `{type-string}`: The associated value

#### `:get_string(key)`

Retrieves the value associated with a key & coerces to string.

**Returns:**

- `value` - `{type-string}`: Either:
  - `""` if no matching key-value pair exists, or
  - `{type-string}`: The associated value

#### `:get_int(key)`

Retrieves the value associated with a key & coerces it to an integer.

**Returns:**

- `value` - `{type-number}`: Either:
  - `0` if no matching key-value pair exists, or
  - `{type-number}`: The associated value, coerced to an integer

#### `:get_float(key)`

Retrieves the value associated with a key & coerces it to a floating-point number.

**Returns:**

- `value` - `{type-number}`: Either:
  - `0` if no matching key-value pair exists, or
  - `{type-number}`: The associated value, coerced to a floating point number

### Setters

**Arguments & Returns:**

Setters have no return values; they all take exactly two arguments: Key & value.

- `key` - `{type-string}`: the key/name
- `value` - depends on the setter: the value

#### `:set_string(key, value)`

**Arguments:**

- `value` - `{type-string}`: The value to associate with `key`. Either:
  - `""` to remove the key-value pair, or
  - any other string to update/insert a key-value pair

#### `:set_int(key, value)`

**Arguments:**

- `value` - `{type-number}`: The integer value to coerce to a string & associate with `key`

{{< notice warning >}}
Integer refers to a C(++) `int` as internally used by the implementation - usually 32 bits wide - meaning it is unable to represent as large integer numbers as the Lua number type. Be careful when storing integers with large absolute values; they may overflow. Keep `value` between `-2^31` and `2^31 - 1`, both inclusive.
{{< /notice >}}

#### `:set_float(key, value)`

**Arguments:**

- `value` - `{type-number}`: The floating-point value to coerce to a string & associate with `key`

{{< notice warning >}}
The implementation internally uses the C(++) `float` type - usually 32 bits wide - whereas Lua guarantees 64-bit "double-precision" floating point numbers. This may lead to a precision loss. Large numbers in particular may be hardly representable.
{{< /notice >}}

#### `:equals(other)`

**Arguments:**

- `other` - MetaData: a MetaData object

**Returns:**

- `same` - `{type-bool}`: whether `self` has the same key-value pairs as `other`

#### `:to_table()`

Converts the metadata to a Lua table representation.

**Returns:**

- `value` - `nil` or `{type-table}`: Either:
  - `nil` if the metadata is invalid (?), or
  - `{type-table}`: A table representation of the metadata with the following fields:
    - `fields`: Table `{[key] = value, ...}`
    - Additional fields depending on the subclass

{{< notice tip >}}
Use `table = assert(meta:to_table())` to error if the operation failed.
{{< /notice >}}

#### `:from_table(table)`

Sets the key-value pairs to match those of a given table representation or clears the metadata.

**Arguments:**

- `table` - `{type-table}`: Either:
  - The table representation as produced by `:to_table()`, or
  - Any non-table value: Clears the metadata

**Returns:**

- `value` - `{type-bool}`: whether loading the table representation succeeded

{{< notice tip >}}
Use `assert(meta:from_table(table))` to error if the operation failed.
{{< /notice >}}
