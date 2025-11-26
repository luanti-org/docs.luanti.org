---
title: Basic data structures
aliases:
  - /Engine/Basic_data_structures
  - /engine/basic-data-structures
---

# Basic data structures

## IRC logs on this subject

[minetest-delta/2011-11-07](http://web.archive.org/web/20160309052711/http://logs.2pktfkt.de/minetest-delta/2011-11-07.html) [minetest-delta/2011-11-08](http://web.archive.org/web/20160308105531/http://logs.2pktfkt.de/minetest-delta/2011-11-08.html)

For the log dating 2011-11-07, the subject discussion starts at the end.

## Node

Simply put, [nodes](/for-players/nodes) are the "cubes" that make the world.

A node in Luanti is a cubic section of the world with the size 1×1×1 set in a 3-D raster. Each node has one, and only one distinct type like dirt, sand, stone, air, water source, etc. Every node in the world has whole-number coordinates like (4,6,12). This means you can't place a node at a position like (4.5, 7.553, -64.5). There can only be one node per 3-D position, e.g. there can't be a stone _and_ sand node at the same position.

Nodes are most recognizable as the cubes that make the world, like blocks of dirt, sand or stone, but this is a simplification, because things that don't _look_ like a cube (like stairs, slabs, plants, and even air (=empty space) are considered nodes, too. Nodes in Luanti are the equivalent to blocks in Minecraft.

Although nodes themselves don't overlap with each over in a technical sense, their _visual appearance_ and even the collision properties of individual nodes _can_ be larger than 1×1×1, thus leading to a possible overlap in a practical sense. Games sometimes use this for walls with a collision box that extends a little above them to prevent jumping over walls, or to create very large plants or underwater plants (with the `plantlike_rooted` drawtype), and more.

In the Luanti code, a node (represented by the C++ class `MapNode`) contains only a small amount of data that can be transferred easily between the client and the server. This data includes what type of node it is, e.g. Dirt node or Sand node, and some other data for lighting (via param1) and miscellaneous parameters, like rotation, color, etc. (via param2). Nodes can also have metadata attached to them (which allows for much more data) which is commonly used for formspecs but also for other more complex custom stuff.

Nodes don't have any interactive functionality, this is done by the client and the client knows what to do depending on what type of node it is.

Special nodes are ignore, air, and unknown node. These are the only nodes added by Luanti directly (rather than a mod or game).

- Ignore: Where map is not generated, there's ignore. `core.get_node` gives ignore also if the map is not loaded, so to get the node in unloaded chunks you can use LuaVoxelManipulator.
- Air: Air is set when a node is removed. For the player it's empty space.
- Unknown Node: This node is a special case when the map contains a node with an unknown/undefined type.

## Block / MapBlock

A block (or MapBlock) in Luanti, is a collection of 16x16x16 nodes (described above), and they are the pieces that together make the world/map.

In code, the block (represented by the class MapBlock) contains one mesh (only client-side) for all 16x16x16 nodes. A mesh is the geometry of an object, in this case the block. A mesh contains a collection of materials aka. textures, which is how each node is represented visually.

The map block is saved like this:

```
// This describes version 22 and higher

u8 flags; // is_underground:0x1, m_day_night_differs:0x2, !m_generated:0x8
u16 m_lighting_complete; // since version 27
u8 content_width; // can only be 1 or 2
u8 params_width; // always set to 2
bulk_data; // compressed
metadata; // serialized and compressed
// disk only
u8 unused_zero; // only version 23
node_timers; // only version 24
objects;
u32 timestamp;
NameIdMapping nimap; // node ids in this block
node_timers; // since version 25

// bulk_data (after decompression):
u8 param0[nodecount]; // only if content_width=1, param0 contains the content
	// nodecount is 16*16*16
u16 param0[nodecount]; // only if content_width=2
u8 param1[nodecount]; // the light values
u8 param2[nodecount]; // only if content_width=1
	// param2 may contain param0 data in the upper 4 bits:
	// if param0[i] > 0x7f (which is 127) then
		// u16 param0[i] := (param0[i] << 4) | (param2[i] >> 4)
		// param2[i] := param2[i] & 0xf
u8 param2[nodecount]; // only if content_width=2


```

## mapchunk

A mapchunk in Luanti is a collection of 5x5x5 MapBlocks (described above), and each time map generates, another mapchunk appears.

## Map

The Map (class Map) stores MapBlocks and works as an abstraction layer for the MapBlocks, allowing the users of Map to see the world as a consistent bunch of MapNodes.

Map also knows how to load and save MapBlocks.

It also contains functionality for lighting and liquid updates.

## VoxelManipulator

The VoxelManipulator is an object that stores arbitrary areas from the Map, for transferring into other threads and allowing faster node access than the Map does.

## See also

- [Glossary](/glossary)
