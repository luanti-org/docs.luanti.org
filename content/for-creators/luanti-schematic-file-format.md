---
title: Luanti Schematic File Format
aliases:
  - /Minetest_Schematic_File_Format
  - /luanti-schematic-file-format
  - /content-dev/luanti-schematic-file-format
---

# Luanti Schematic File Format

The **Luanti Schematic File Format** if the file format used by Luanti when schematics are serialized by e.g. `core.create_schematic`. Schematic files are supposed to have “`.mts`” as a file name suffix. This document describes the MTS format native to Luanti.

## Informal definition

Warning: This definition has not been proofread yet! Don't rely on it 100% yet.

### Overview

An MTS file has 3 sections, in this order:

- Header
- Name ID Table
- Node Definitions Section

All values are stored in _big endian_ format.

| Offset  | Length | Description                              |
|---------|--------|------------------------------------------|
|       0 |      4 | magic number, ASCII letters "MTSM"       |
|       4 |      2 | file format version, currently 4         |
|       6 |      2 | size X                                   |
|       8 |      2 | size Y                                   |
|      10 |      2 | size Z                                   |
|      12 |      Y | layer probability values                 |
|    12+Y |      2 | number of strings in Name ID Table       |

Layer probability values: This is a sequence of unsigned 8-bit numbers (0-255) for each Y layer. Each Y layer has a chance of appearing with the given probability. The probability ranges from 0 to 127, with 0 meaning 0% probability and 127 meaning 100% probability. Bit 7 is reserved and must be 0, meaning that values greater than 127 should be avoided.

The header is followed by the Name ID Table, which is followed by the Node Definitions section.

### Name ID Table

For each string, the following record format repeats:

| Offset | Length | Description              |
|--------|--------|--------------------------|
|      0 |      2 | length of the string (N) |
|      2 |      N | string, node itemstring  |

The node IDs in the next section reference these.

### Node Definitions Section

This part of the file is zlib compressed, with the deflate algorithm using gz header bytes [RFC 1950](http://tools.ietf.org/html/rfc1950), but not with the gzip header which has magic bytes too. After uncompressing, the format is as follows:

| Offset  | Length     | Description                              |
|---------|------------|------------------------------------------|
|       0 | 2*X*Y*Z    | node IDs (param0)                        |
| 2*X*Y*Z | X*Y*Z      | probability values (param1)              |
| 3*X*Y*Z | X*Y*Z      | param2                                   |

To get the node ID from `param0` for a given coordinate (x,y,z), you should calculate the index `param0[(Z-z)*Z*Y + y*X + x]`. That's right, the Z axis is mirrored.

Each node ID is stored on a big endian 2 bytes. Orientation is **not** defined in any way, there's absolutely no convention.

Probability values in `param1` are encoded in unsigned 8 bits. They are indexed the same way as node IDs. Probability ranges from 0 (0%) to 127 (100%). Bit 7 means force node placement, i.e. the node will be able to replace non-air nodes as well. (In legacy version 3, `param1`'s probability range was from 0 to 0xFF, there's no force placement.)

`param2` is an 8-bit value (0-255), the meaning depends on the node definition. See `lua_api.md` to learn more about `param2` (keywords: “param2”, “paramtype2”).

## Official definition

The official definition of version 4 of this file format follows and has been copied from [/src/mapgen/mg_schematic.h](https://github.com/luanti-org/luanti/blob/5.1.0/src/mapgen/mg_schematic.h) in the source code of version 5.1.0.

```
All values are stored in big-endian byte order.
[u32] signature: 'MTSM'
[u16] version: 4
[u16] size X
[u16] size Y
[u16] size Z
For each Y:
	[u8] slice probability value
[Name-ID table] Name ID Mapping Table
	[u16] name-id count
	For each name-id mapping:
		[u16] name length
		[u8[]] name
ZLib deflated {
For each node in schematic:  (for z, y, x)
	[u16] content
For each node in schematic:
	[u8] param1
	  bit 0-6: probability
	  bit 7:   specific node force placement
For each node in schematic:
	[u8] param2
}
```

- Source code 5.1.0: [/src/mapgen/mg_schematic.h](https://github.com/luanti-org/luanti/blob/5.1.0/src/mapgen/mg_schematic.h)
- Source code bleeding edge: [/src/mapgen/mg_schematic.h](https://github.com/luanti-org/luanti/blob/master/src/mapgen/mg_schematic.h)

## Version changes

As of version 5.1.0, the current version of the MTS file format is 4.

- `1`: Initial version
- `2`: Fixed messy never/always place; the probability of `0` is now never, and `0xFF` is always
- `3`: Added y-slice probabilities; this allows for variable height structures
- `4`: Compressed range of node occurrence probability, added per-node force placement bit (used since 0.4.16 or earlier)
