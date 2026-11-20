---
title: Map generator
aliases:
  - /Map_generator
  - /Map-Database
  - /Mapgen
  - /mapgen
---

The **map generator** (“mapgen” for short) is the Luanti component that procedurally generates the world around the player.

The Luanti engine provides several built-in map generators implemented in C++. It is possible to choose between them when creating a world. Most of them are based on Perlin noises: functions which allow associating to each point a random yet consistent value.

Additionally, you can [write your own mapgen in Lua](/for-creators/mapgen/custom-lua-mapgen), based on the `singlenode` built-in map generator that generates an empty world.

## Overview

These are the available mapgens described in brief:

- `v5`: Unrealistic yet interesting landscape with frequent floating blobs, extreme terrain overhangs, and deep oceans. The weirdness has an appeal to some
- `v6`: Simple hills, cliffs and plateaus, rather flat and small-scale mapgen, no terrain overhangs, predefined biomes
- `v7`: Very widespread landscape, tall mountains with overhanging terrain and far plains. Frequent large water canals at sea level
- `carpathian`: Complex and realistic mountains with multiple variants, sometimes reaching extreme heights, separated by oceans and far-reaching low and mostly flat plains
- `valleys`: Many hills, mountains and valleys containing river water flowing downhill
- `flat`: A perfectly flat world, but with caves, biomes and decorations
- `fractal`: Map is based on a fractal of your choice; highly configurable
- `singlenode`: Empty world, used to implement custom map generators in Lua

All map generators use so-called "mapgen aliases". These are node aliases defined by games to choose some basic nodes used by the mapgen. This includes stone (`mapgen_stone`), water source (`mapgen_water_source`) and river water source (`mapgen_river_water_source`). Most other nodes used by the mapgen are specified via the Biome API instead. The exception is `v6`, where all nodes are defined using mapgen aliases.

All map generators have biomes. In all map generators except `v6`, these biomes are defined by games and mods using the Biome API. If no biomes are defined, the result will be a stone-only landscape. The `v6` map generator is special because its biomes are completely predefined and can't be changed trivially.

All map generators allow configuration in the advanced settings. A list of all map generator settings can also be found in [minetest.conf.example](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example).

### Gallery

Here are examples of landscapes generated with the different map generators:

- `v5` in Minetest Game

  [![](/images/mapgen/Mapgen_v5.jpg)](/images/mapgen/Mapgen_v5.jpg)

- `v6` in Minetest Game

  [![](/images/mapgen/Mapgen_v6.jpg)](/images/mapgen/Mapgen_v6.jpg)

- `v7` in Minetest Game

  [![](/images/mapgen/Mapgen_v7.jpg)](/images/mapgen/Mapgen_v7.jpg)

- `carpathian` in Minetest Game

  [![](/images/mapgen/Mapgen_carpathian.jpg)](/images/mapgen/Mapgen_carpathian.jpg)

- `valleys` in Minetest Game

  [![](/images/mapgen/Mapgen_valleys.jpg)](/images/mapgen/Mapgen_valleys.jpg)

- `flat` in Minetest Game

  [![](/images/mapgen/Mapgen_flat.jpg)](/images/mapgen/Mapgen_flat.jpg)

- `fractal` in Minetest Game

  [![](/images/mapgen/Mapgen_fractals_fractal_1.jpg)](/images/mapgen/Mapgen_fractals_fractal_1.jpg)

- `singlenode` in Minetest Game

  [![](/images/mapgen/Mapgen_singlenode.jpg)](/images/mapgen/Mapgen_singlenode.jpg)

## Description

All map generators described in full detail.

To learn more about detailed configuration, see [Map generator features](/for-creators/mapgen/features).

### v5

Notable for its unique and somewhat strange terrain shape and occasional floating islands which often look like blobs. Extreme overhanging terrain is not uncommon and the landscape is sometimes realistic, sometimes strange and challenging. The oceans can sometimes reach extreme depths as well. While the landscape is not exactly realistic, this mapgen is intentionally included in Luanti because its uniqueness and weirdness also has a special appeal to some.

Like most map generators, v5 generates many caves in the underground. The caves are often tight but may be broad at times. They can sometimes be very long, deep and tricky to explore and form a complex tunnel system. Deep in the underground, there is a low chance to encounter giant caves. Like in most other mapgens, lava and water lakes may appear as lakes in the underground, but lava starts to appear very deep at Y=-273 or below.

Landscapes are based on 3D Perlin noise.

### v6

Notable for rather flat and simple hills and cliffs. The landscape is rather simple, the hills don't extend above Y=47 and there are no natural terrain overhangs (ignoring caves). The weirdness of v5 is eliminated.

The caves are simple, short and tight and have little variation, but they are frequent. Larger caves only generate for underground water and lava lakes. There are no giant caves. This is the only map generator in which lava is already generated below Y=-32, much higher than in the other mapgens.

This is the only map generator with predefined biomes: Grassland, forest, jungle, desert, taiga, tundra. These biomes can't be modified by games or mods.

It's also the only map generator with predefined decorations: Regular tree, apple tree, jungle tree and pine tree. These can be disabled using the `trees` v6 mapgen flag, the generic `decorations` mapgen flag doesn't affect them. Even in v6, it's still possible to register custom decorations using the Lua API.

Other unique features include: Large patches of dirt and sand in the underground, rare gravel fields on the surface and a setting to generate a flat map in v6 style.

Generated entirely using 2D Perlin noise.

### v7

Generates a very large-scale environment with large biomes and plains. One of the most unique features in this map generator are the broad and deep water canals (called “ridges”) at sea level, but they can be disabled. This mapgen also has the unique feature of supporting floating islands high in the sky (see [Map generator features](/for-creators/mapgen/features)), but they are disabled by default.

The generated caves are broad, often have lots of space and are often very long and complex and like to branch off. Sudden drops are not unusual. Deep in the underground, giant caves may form (like in v5).

Uses 2D and 3D Perlin noise. This mapgen is the default selection since version 0.4.15.

### carpathian

Vast and relatively flat plains, rolling hills and complex, realistic mountain ranges dominate the landscape. Mountains come in several forms. In mountain ranges, ridges and terraced mountains can form. Rarely there are extremely big mountains and rarely there are fjords.

### valleys

Generates a landscape featuring many hills, mountains and valleys. The valleys often contain rivers with river water. The rivers are very different than in v7, since they are not at ocean level and actually flow downhill. Another unique feature of this mapgen is the altitude chill. The biome temperature falls with the height which means that at high elevations, cold biomes are much more likely.

The caves are similar to those of v7, but they tend to be much larger on average.

### flat

Generates a perfectly flat world (ignoring structures and caves) with biomes. It can be configured to add occasional hills and lakes.

The generated caves are practically identical to those of v7.

### fractal

Generates a map based on a fractal. It creates by far the weirdest terrain shapes, but its results are mostly predictable.

By default, a map based on the Mandelbrot set is generated. It is possible to choose one of many fractals which are based on the Mandelbrot and Julia set, which is chosen in the advanced settings (technical setting name “`mgfractal_fractal`”). Changing this setting will yield wildly different results; if you want to make most of this mapgen, make sure to play around with the advanced settings a bit.

The generated caves are practically identical to those of v7.

### singlenode

Generates an empty world.

To be precise: By default, this produces a world with only [Air](/for-players/nodes/#air) everywhere. For games, it's possible to choose a different node by defining the `mapgen_singlenode` mapgen alias.

It is intended to be used for mapgen mods which define their own map generation from scratch. This mapgen is not really useful if left unmodified.

## See also

- [Stability of each mapgen](https://forum.luanti.org/viewtopic.php?f=18&t=19132), a post by paramat on the forums describing which map generators are "officially stable" (last updated 2019)
- [Map generator features](/for-creators/mapgen/features)
- [Map Generator Evolution](/about/history/mapgen-evolution)
