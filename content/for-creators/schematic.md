---
title: Schematic
aliases:
  - /Schematic
  - /schematic
  - /content-dev/schematic
---

# Schematic

**Schematics** are pre-defined node patterns to be placed somewhere in the world. They allow to create some complex figures or structures and repeat them with little random alterations.

A schematic tells in an area what nodes should be created, with a given probability for each node to appear.

## Features

Schematics save the contents of an area of the world. It saves node names and the param2 value for each node. Node metadata is not saved.

Additional, schematics have the following features:

- Specify a probability for every node to be placed (or not be placed)
- Specify a probability for each Y layer to be placed (or be removed entirely)
- Set certain nodes to be force-placed (thus overwriting any node when the schematic is placed)

## Schematic specifier

Functions that use schematics are passed a schematic specifier. A specifier can have 3 forms:

- A .mts file name as a string
- A raw data table
- A registered schematic identifier as a number, returned by core.register_schematic

## Schematic file

The MTS file format specifies a Luanti schematic in binary format. For information about the format see [Luanti Schematic File Format](/for-creators/luanti-schematic-file-format/).

The [Schematic Editor](https://content.luanti.org/packages/Wuzzy/schemedit/) mod allows you to conveniently export and import MTS schematic files without leaving the engine. The engine also has support for creating schematics through the Lua API with [core.create_schematic](https://api.luanti.org/core-namespace-reference/#schematics).

In addition to this there is the external program [MTSEdit](https://gitlab.com/bztsrc/mtsedit) where you can edit schematics in an isometric view.

## Schematic table

The schematic table has 2 mandatory attributes and 1 optional attribute.

- size: the bounding box in nodes, a 3D vector
- data: the flat list of MapNode to write

The data list is ordered in z, y, x. Than means for a 3x3x3 box it will be a list of 27 MapNodes.

- The first 3 are positioned at the stone block, at the node between and at the blue block,
- The next 3 are just above the 3 previous,
- The next is at the green block and the 2 next going toward the blue box, on top,
- The next 9 are the same pattern one node toward the red block,
- And finally the next 9 are the same pattern starting at the red block

Each MapNode holds the node name and the probability to appear. See [Schematic specifier in lua_api.md](https://github.com/luanti-org/luanti/blob/master/doc/lua_api.md#schematic-specifier) for more details.

A minimal table for a 3x3 bush sample:

```lua
local my_schematic = {
	size = {x = 3, y = 3, z = 3},
	data = {
		-- The side of the bush, with the air on top
		{name = "default:bush_leaves"}, {name = "default:bush_leaves"}, {name = "default:bush_leaves"}, -- lower layer
		{name = "default:bush_leaves"}, {name = "default:bush_leaves"}, {name = "default:bush_leaves"}, -- middle layer
		{name = "air"}, {name = "air"}, {name = "air"}, -- top layer
		-- The center of the bush, with stem at the base and a pointy leave 2 nodes above
		{name = "default:bush_leaves"}, {name = "default:bush_stem"}, {name = "default:bush_leaves"}, -- lower layer
		{name = "default:bush_leaves"}, {name = "default:bush_leaves"}, {name = "default:bush_leaves"}, -- middle layer
		{name = "air"}, {name = "default:bush_leaves"}, {name = "air"}, -- top layer
		-- The other side of the bush, same as first side
		{name = "default:bush_leaves"}, {name = "default:bush_leaves"}, {name = "default:bush_leaves"}, -- lower layer
		{name = "default:bush_leaves"}, {name = "default:bush_leaves"}, {name = "default:bush_leaves"}, -- middle layer
		{name = "air"}, {name = "air"}, {name = "air"}, -- top layer
	}
}
```

You can then provide `my_schematic` every time a Schematic specifier is requested, for example in `core.register_decoration`.

## Placing schematics

Schematics are placed either with core.place_schematic or at world generation with `core.register_decoration`.

When using `core.register_decoration`, be aware that the decoration is placed inside a ground node and not on top, unlike simple decorations. You may want to add one layer for the roots of the schematic. See also in lua_api.md about the documentation of `core.register_decoration` for the flags to center the schematic on certain axes instead of placing it from a corner. Notice that you can only place the schematic from the center or the start of every axis and not an arbitrary offset.

When using `core.place_schematic`, you can provide the offset manually by changing the reference pos.

## Incomplete

Missing some explanations about `yslice_prob` against individual node probability
