---
title: How to emulate mapgen v6 using a customized mapgen v7
aliases:
  - /How_to_emulate_mapgen_v6_using_a_customised_mapgen_v7
  - /mapgen/emulate-mapgen-v6-using-a-customised-mapgen-v7
---

Some players like aspects of [Mapgen](/for-creators/mapgen) v6, but the biomes are hardcoded, so it's not possible for games to register their own biomes. It is also very different from the other mapgens in how it works and is more difficult to support in games.

It is actually possible to approximately emulate Mapgen v6 in Mapgen v7 using customized settings. This is only possible in version 5.2.0 or later due to the “randomwalk” caves now being more configurable, allowing Mapgen v7 to have caves like in Mapgen v6.

## How to emulate mapgen v6

First, copy and paste the lines below into your [`minetest.conf`](/for-players/minetest-conf) file before starting a new Mapgen v7 world:

```
mgv7_spflags = nomountains, noridges, nofloatlands, nocaverns
mgv7_cave_width = 10
mgv7_large_cave_depth = 47
mgv7_small_cave_num_min = 24
mgv7_small_cave_num_max = 24
mgv7_large_cave_num_min = 2
mgv7_large_cave_num_max = 2
mgv7_np_terrain_base = {
  offset = 21.0,
  scale = 16.0,
  spread = (500, 500, 500),
  seed = 85039,
  octaves = 5,
  persistence = 0.6,
  lacunarity = 2.0,
  flags = eased
}
mgv7_np_terrain_alt = {
  offset = -3.0,
  scale = 20.0,
  spread = (250, 250, 250),
  seed = 82341,
  octaves = 5,
  persistence = 0.6,
  lacunarity = 2.0,
  flags = eased
}
mgv7_np_terrain_persist = {
  offset = 0.6,
  scale = 0.0,
  spread = (2000, 2000, 2000),
  seed = 539,
  octaves = 1,
  persistence = 0.6,
  lacunarity = 2.0,
  flags = eased
}
mgv7_np_height_select = {
  offset = -19.5,
  scale = 100.0,
  spread = (250, 250, 250),
  seed = 4213,
  octaves = 5,
  persistence = 0.69,
  lacunarity = 2.0,
  flags = eased
}
mgv7_np_filler_depth = {
  offset = 4.0,
  scale = 2.0,
  spread = (200, 200, 200),
  seed = 91013,
  octaves = 3,
  persistence = 0.55,
  lacunarity = 2.0,
  flags = eased
}

```

Or you can set these parameters manually in the advanced settings, but that will take much longer to do.

Then just create a new v7 world. Old v7 worlds will not be affected.

If you want to return to normal, just delete these lines from `minetest.conf` again.

## Limitations

The emulation is only approximate, it differs in the following ways:

- There is no “mudflow” erosion
- The cliff/slope transitions between the lowland and highland do not have a wide range of "steepness"es, they are now all steep cliffs
- The number of small and large caves per mapchunk is fixed and not varied by noise or biome
- Using the same world seed as a Mapgen v6 world will probably not result in hills, lakes, cliffs, caves, dungeons etc. being in the same locations, even though the character and elements of the mapgen will be similar
- When using Minetest Game, lava is found below Y = -256 and ores are deeper, just like all non-v6 mapgens

---

This page is based on a [forum post by paramat](https://forum.luanti.org/viewtopic.php?f=3&t=24831).
