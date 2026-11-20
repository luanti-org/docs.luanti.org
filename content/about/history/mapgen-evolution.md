---
title: Map generator evolution
aliases:
  - "/Map_Generator_Evolution"
  - /mapgen/evolution
---

This page shows the development history of the [map generators](/for-creators/mapgen) in Luanti.

## Development of a single map generator

Initially, Luanti (then called “Minetest-c55”, later “Minetest”) had only one map generator which was continuously improved and had its own version number, where each version replaced the previous one. The earliest map generators are based on 2D Perlin noise.

### Early development

Also dubbed “Version 0”, this section summarizes the earliest map generator experiments before the release of the first official map generator.

#### minetest-c55-101010000530

_The first mapgen_  
[![](/images/mapgen/First_mapgen.png)](/images/mapgen/First_mapgen.png)

In the very first known version of Minetest-c55, there was just flat terrain made out of grass blocks with stone pyramids on top. There were a few holes. There were no other blocks or caves. The terrain was always the same.

#### minetest-c55-101017205222

_Undefinable terrain_  
[![](/images/mapgen/Earliest_terrain.png)](/images/mapgen/Earliest_terrain.png)

The terrain was no longer flat and the pyramids are gone. The terrain generation was horrible, but at least terrain could be generated. It consisted of grass blocks and stone at surface. Very simple square-ish caves could be found below the surface. The terrain was still not random.

#### minetest-c55-101024231736

_First water generation_  
[![](/images/mapgen/First_water_mapgen.png)](/images/mapgen/First_water_mapgen.png)

Water has been added and forms oceans at a fixed sea level.

#### minetest-c55-101101170627 (and later)

_The first trees_  
[![](/images/mapgen/Puita_jee.png)](/images/mapgen/Puita_jee.png)

_More trees_  
[![](/images/mapgen/Mapgen_updated.png)](/images/mapgen/Mapgen_updated.png)

_Even more trees_  
[![](/images/mapgen/Grumbuduts_ymparisto.png)](/images/mapgen/Grumbuduts_ymparisto.png)

Trees with leaves were added in, but they were very basic. Bushes (a single leaves block on the ground) were added as well. These primitive bushes have been removed in a later version.

### Version 1

_Not mapgen version 1, but very close to it._  
[![](/images/mapgen/Mapgen_v1.png)](/images/mapgen/Mapgen_v1.png)

The first official mapgen was introduced in Minetest-c55 0.0.1. Features:

- Simple, **random** terrain with stone and dirt with grass
- Dirt generates below water and at some beaches
- Trees (consisting of Tree and Leaves)
- Simple lakes and oceans at a fixed sea level
- Simple caves and ravines (vertical caves)
- First ores: Mese blocks and coal ores are generated underground

### Version 2 dev3

_Mapgen 2 dev3_  
[![](/images/mapgen/Mapgenv2_3.jpeg)](/images/mapgen/Mapgenv2_3.jpeg)

Mountains were made more pronounced, ore generation was better and trees have been improved. Although this was a development version.

### Version 2 dev4

_Mapgen 2 dev4_  
[![](/images/mapgen/Mapgenv2_4.jpeg)](/images/mapgen/Mapgenv2_4.jpeg)

Things were tuned a little better.

### Version 2

_Mapgen 2_  
[![](/images/mapgen/Scenery2.jpeg)](/images/mapgen/Scenery2.jpeg)

Sand has been added and formed sand beaches. A simple mud flow algorithm smoothens out dirt / dirt with grass hills. The cave generation has seen notable improvements and iron ore has been added as well.

### Version 3 "3d noise"

_A giant cliff_  
[![](/images/mapgen/Turbulence_cliff_test.jpeg)](/images/mapgen/Turbulence_cliff_test.jpeg)

3D Perlin noise is used to generate this terrain.

### Version 4

_Mapgen 4 biome_  
[![](/images/mapgen/Mapgenv3_4.jpeg)](/images/mapgen/Mapgenv3_4.jpeg)

More tweaking.

### Version 5

_Historic mapgen version 5 in Minetest-c55 0.3.0_  
[![](/images/mapgen/Minetest_mapgen5_funky.jpeg)](/images/mapgen/Minetest_mapgen5_funky.jpeg)

Official map generation for version 0.3.1. It still uses 3D Perlin noise like the previous versions and is iconic for its extreme and often “blobby” terrain. In the 0.3 versions, this map generator hat predefined biomes like the current v6 map generator. In Minetest-c55 0.4, it was removed, but has been revived later (see [v5](#v5)).

This version of the map generator is also used in Voxelands, a fork of Minetest-c55.

### Version 6

_Mapgen version 6 in an early version of Minetest-c55 0.4_  
[![](/images/mapgen/Screenshot_2438759180_mapgenv6.jpeg)](/images/mapgen/Screenshot_2438759180_mapgenv6.jpeg)

_Plains biome in mapgen version 6 (before Minetest-c55 0.4.3) with visible "terrain lines" which were fixed later_  
[![](/images/mapgen/Plains.png)](/images/mapgen/Plains.png)

_Mountains in mapgen version 6, before Minetest 0.4.7_  
[![](/images/mapgen/Mountain.png)](/images/mapgen/Mountain.png)

Version 6 of the map generator was introduced in Minetest-c55 0.4.0 to replace the map generator version 5. This version uses 2D Perlin noise again and the terrain is now much smoother and more realistic than of the previous version. Map generation has also become much faster.

This version introduces jungles, deserts and rare gravel fields.

Version 6 was the last version of the only map generator before support for multiple map generators has been added. This mapgen has then directly be continued under the name “[v6](#v6)”.

## Development of multiple map generators

Since Minetest 0.4.6, players are able to choose one of multiple map generators, with v6 being the default selection initially. Since then, more and more map generators have been added and existing map generators have been improved over time. From this point on, there is no longer a single “version” of the map generator and the concept of a map generator “version” no longer applies. Each map generator is now developed independently. It is important to understand that the mapgens v5 and v6 should not be viewed as inferior to v7, instead they should just be viewed as different mapgens. “v5”, “v6” and “v7” are now just names, not version numbers. All mapgens currently included in Minetest regularly receive bugfixes and new features.

In Minetest 0.4.15, v7 became the new default selection.

### v6

_Mapgen v6 in Minetest 0.4.13-dev_  
[![](/images/mapgen/Mapgen_v6.jpg)](/images/mapgen/Mapgen_v6.jpg)

The mapgen “v6” (also known as “mgv6”) is the direct continuation of [version 6](#Version_6) (before support for multiple mapgens has been added) and has pretty much the same features. In Minetest 0.4.13, support for snow biomes and ice sheets has been introduced to v6. This mapgen it is still officially supported in Minetest, it will not be removed anytime soon and improvements and bugfixes are still applied from time to time.

### singlenode

_singlenode mapgen in Minetest 0.4.13_  
[![](/images/mapgen/Mapgen_singlenode.jpg)](/images/mapgen/Mapgen_singlenode.jpg)

This mapgen was introduced in Minetest 0.4.6 by [celeron55](/about/history/celeron55). It creates a world with only Air (can be changed with configuration) and is intended to be used by games and mods for making custom map generators, so they can start completely from scratch. The name “singlenode” means that it generates a _single_ type of _node_ everywhere, [Air](/for-players/nodes/#air) by default.

This map generator has been hidden from the map generator selection screen for a couple of versions up to 0.4.16, in which it has been re-enabled again.

### indev

This mapgen was introduced in Minetest 0.4.6 by proller and contained many experimental features. It is based on v6. Major new features included floating islands at Y=500 and above, rare huge caves, and a more extreme terrain near the world boundaries in form of larger biomes, higher mountains and deeper oceans. It has been removed in version 0.4.10 because it was considered to be of low quality.

- _Most of the surface in the indev mapgen is almost identical to v6 (Minetest 0.4.9)_  
  [![](/images/mapgen/Mapgen_indev_spawn.jpg)](/images/mapgen/Mapgen_indev_spawn.jpg)

- _Floating islands in indev mapgen, Minetest 0.4.9_  
  [![](/images/mapgen/Mapgen_indev_floating_islands.jpg)](/images/mapgen/Mapgen_indev_floating_islands.jpg)

- _Extreme terrain and a world boundary in indev mapgen, Minetest 0.4.9_  
  [![](/images/mapgen/Mapgen_indev_edge.jpg)](/images/mapgen/Mapgen_indev_edge.jpg)

- _High mountains near the world boundaries in indev mapgen, Minetest 0.4.9_  
  [![](/images/mapgen/Mapgen_indev_edge_mountains.jpg)](/images/mapgen/Mapgen_indev_edge_mountains.jpg)

- _A huge (and buggy) cave in indev mapgen, Minetest 0.4.9_  
  [![](/images/mapgen/Mapgen_indev_huge_cave.jpg)](/images/mapgen/Mapgen_indev_huge_cave.jpg)

### v7

_Mapgen v7 in Minetest 0.4.9 without biomes_  
[![](/images/mapgen/Mapgen_v7_0_4_9.jpg)](/images/mapgen/Mapgen_v7_0_4_9.jpg)

_Mapgen v7 in Minetest 0.4.13-dev with biomes_  
[![](/images/mapgen/Mapgen_v7.jpg)](/images/mapgen/Mapgen_v7.jpg)

_Floatlands of mapgen v7 in Minetest 0.4.15_  
[![](/images/mapgen/Mapgen_v7_floatlands.jpg)](/images/mapgen/Mapgen_v7_floatlands.jpg)

Also called “mgv7”. It was introduced in Minetest 0.4.7 by kwolekr, in which the map generator was still unusable. This map generator uses a combination of 2D and 3D Perlin noise. It is notable for its large rivers at Sea level. Caves in v7 are also very different than in v6, they are generally much larger.

This mapgen has a long history of development and has seen countless improvements over the Minetest versions.

- Initially, it only created worlds mostly made out of stone. Biomes for v7 were eventually added to Minetest Game at a later point
- In Minetest 0.4.13, many big improvements to the terrain generation and noise parameters have been made; sandstone and desert stone dungeons have been added as well
- In Minetest 0.4.14, the Biome API was introduced for v7 which makes v7 the first map generator which supports the Biome API, i.e. biomes defined by mods. The days of the stone worlds are over. Also, caves have been improved
- In Minetest 0.4.15, v7 became the default selected mapgen. Optional and experimental support for huge floating islands at Y=1280 and above was added, but is turned off by default. River channels can also carve through mountains

### math

_“mandelbox” mode of the math mapgen in Minetest 0.4.9_  
[![](/images/mapgen/Mapgen_math_mandelbox.jpg)](/images/mapgen/Mapgen_math_mandelbox.jpg)

_“mengersponge” mode of the math mapgen in Minetest 0.4.9 with lighting bugs_  
[![](/images/mapgen/Mapgen_math_mengersponge.jpg)](/images/mapgen/Mapgen_math_mengersponge.jpg)

The mapgen “math” was introduced in Minetest 0.4.8 by proller. This was really a collection of 3 different map generators based on simple maths. It was capable of creating Menger sponges, mandelboxes and spheres. Mods could adds biomes with Minetest's biome API, but this was never used in Minetest Game, so only stone worlds were created by default.

This map generator has been removed in Minetest 0.4.10 when proller left the project because it was considered to be of very low quality and had many obvious lighting problems (see [forum topic](https://forum.luanti.org/viewtopic.php?f=3&t=7256#p148920)).

The math mapgen is being continued in Freeminer, a fork of Minetest.

### v5

_Modernized mapgen v5 in Minetest 0.4.13-dev_  
[![](/images/mapgen/Mapgen_v5.jpg)](/images/mapgen/Mapgen_v5.jpg)

Also called “mgv5”. This mapgen is a revival of the [historic map generator version 5](#Version_5) from Minetest-c55 0.3. In Minetest 0.4.11, this historic map generator has been revived by paramat because of its iconic terrain shape.

But this mapgen is not identical to the historic version 5. The terrain shape is _nearly_ identical to the historic version. The main difference is that the modernized v5 map generator has mod-defined biome support (like v7), rather than predefined biomes. It also shares many other features with v7 which are not related to the terrain, for example desert and sandstone dungeons. Some features which have been added to v7 were often also added to v5.

### fractal

_One of many possible terrains by the fractal mapgen in Minetest 0.4.13-dev_  
[![](/images/mapgen/Mapgen_fractals_fractal_1.jpg)](/images/mapgen/Mapgen_fractals_fractal_1.jpg)

Introduced in Minetest 0.4.13 by paramat. It creates worlds based on fractals, specifically those based on 3D and 4D Mandelbrot and Julia sets, including a mandelbulb. As of version 0.4.15, there are 18 variants which this mapgen supports. This mapgen has a similar intention like the old math mapgen, but it has a very different feature set compared to math.

### flat

_flat mapgen_  
[![](/images/mapgen/Mapgen_flat.jpg)](/images/mapgen/Mapgen_flat.jpg)

The flat map generator was introduced in Minetest 0.4.14 by paramat. It can be used with or without biomes, decorations, caves, dungeons, etc.

Technically, Minetest was able to generate flat maps since 0.4.5 by using a mapgen setting (`mg_flags`). Later this flag has been removed from `mg_flags` and instead became a mapgen setting for v6 (`mgv6_spflags`) to generate flat maps in v6 style.

### valleys

_valleys mapgen in Minetest 0.4.13-dev_  
[![](/images/mapgen/Mapgen_valleys.jpg)](/images/mapgen/Mapgen_valleys.jpg)

Introduced in Minetest 0.4.14 by Duane Robertson. It is notable for its “valley”-like shapes and its flowing rivers. These rivers are very different than in v7: They flow downhill rather than being flat at ocean level.

In Minetest Game, these rivers are made out of River Water, a liquid which has been introduced in the same version for the rivers. It has been introduced because a liquid with a reduced flowing range was needed, as normal Water would easily flow over.

### carpathian

_carpathian mapgen in Minetest 0.5.0-dev-8221d3bc_  
[![](/images/mapgen/Mapgen_carpathian.jpg)](/images/mapgen/Mapgen_carpathian.jpg)

Introduced in the developer version on 6th July 2017 by vlapsley. It has been first showcased [in the forums](https://forum.luanti.org/viewtopic.php?f=9&t=17174). About 1.5 years later, it was finally released in a stable Minetest release: Minetest 5.0.0. This mapgen is notable for its complex mountains (multiple variants: terraced, stepped, very big, ridged) and hills. Vast flat plains, mountains and oceans dominate the landscape. Fjords are also possible, but rare.
