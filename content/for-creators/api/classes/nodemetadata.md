---
title: NodeMetaData
aliases:
  - /api/classes/nodemetadata
---

# NodeMetaData

NodeMetaData allows extending the basic node storage with a per-world, per-in-world-node persistent string key-value store implementing all methods of MetaData allowing you to associate arbitrary data with any node. Additionally, node metadata allows storing inventories for nodes like chests or furnaces.

The traditional node storage in Luanti is usually limited to:

- A content ID (determining the node name),
- The `param1` unsigned byte (`0` - `255` both inclusive) which is usually reserved for lighting,
- The `param2` unsigned byte (`0` - `255` both inclusive) which can be used depending on `paramtype2`

## `core.get_meta(pos)`

Obtains a mutable NodeMetaData reference for the node at `pos`.

**Arguments:**

- `pos` - Vector: Node position on the map; floats are rounded appropriately.

**Returns:**

- `meta` - NodeMetaData: Corresponding NodeMetaData reference

## `core.find_nodes_with_meta(pos1, pos2)`

Searches a region for nodes with non-empty metadata.

**Arguments:**

- `pos1` - Vector: First corner of the cuboid region (inclusive)
- `pos2` - Vector: Second corner of the cuboid region (inclusive)

Cuboid corners are rounded properly if they are not integers.

**Returns:**

- `positions` - List of Vector: List of integer positions of nodes that have non-empty meta

## Special Fields

### `formspec`

FormSpec to show when the node is interacted with using the "place/use" key (right-click by default).

Has no effect if `on_rightclick` is defined in the node definition (see `core.register_node`).

The most notable advantage of this over calling `core.show_formspec` in `on_rightclick` is client-side prediction:

The client can immediately show the FormSpec; the second approach requires the client to first inform the server of the interaction, to which the server then responds with the FormSpec. This takes one round-trip time (RTT).

The obvious disadvantage is that FormSpecs can't be dynamically generated in response to user interaction; formspecs must be mostly static. To alleviate this, formspecs provide context-dependent placeholders like the `context` or `current_player` inventory locations (see FormSpec).

{{< notice info >}}
The `context` inventory location can only be used in FormSpecs using the special `formspec` NodeMetaData field. FormSpecs shown using `core.show_formspec` must use `nodemeta:<X>,<Y>,<Z>` to reference inventories instead.
{{< /notice >}}

Another disadvantage is that plenty of redundant metadata - often a constant FormSpec - has to be stored with every node. This metadata also has to be sent to clients. Luanti's mapblock compression should be able to compress duplicate substrings - FormSpecs in this case - reasonably well though.

In terms of network traffic it depends: With meta, the FormSpec is "only" sent when the mapblock gets updated;
whereas the other approach re-sends the FormSpec every time the user interacts with the node.

### `infotext`

Text to show when the node is pointed at (same as the `infotext` object property). Long texts are line-wrapped, even longer texts are truncated.

## Methods

### `:mark_as_private(keys)`

NodeMetaData is by default fully sent to clients; the special `formspec` and `infotext` fields triggering client-side behavior obviously need to be sent to clients to work.

All other fields do not need to be sent to clients unless you want to explicitly support local map saving.

{{< notice tip >}}
Mark all other fields as private to reduce traffic.
{{< /notice >}}

If you don't want clients to be able to see private NodeMetaData fields - usually to prevent cheating - you must mark them as private.

{{< notice note >}}
The private marking is tied to a key-value pair.

- If the key-value pair is deleted, the private marking is deleted as well.
- If the key-value pair is recreated, the private marking must be recreated as well.
  {{< /notice >}}

{{< notice note >}}
`to_table` and `from_table` do not keep track of which fields were marked as private.
{{< /notice >}}

**Arguments:**

- `keys` - `{type-string}` or list of `{type-string}`: Either:
  - A single key to mark as private, or
  - A list of keys to mark as private

### `:get_inventory()`

Get the inventory associated with the node.

**Returns:**

- `inv` - Inventory: The inventory associated with the node

### `:to_table()`

Extends MetaData `:to_table()` by additionally adding an `inventory` field for
a table `{[listname] = list}` where `list` is a list of ItemStrings
(`""` for empty) with the same length as the size of the inventory list.

{{< notice tip >}}
Use `table = assert(meta:to_table())` to error if the operation failed.
{{< /notice >}}

### `:from_table(table)`

Extends MetaData `:from_table(table)` to add support for the `inventory` field.
