---
title: World Boundaries
aliases:
  - /World_boundaries
  - /world-boundaries
---

# World Boundaries

Luanti's world is not infinitely large. Therefore, you can theoretically reach the world boundaries if you just travel far enough. How easy it would be for a player to reach the boundary depends on what movement options are provided by the game, or if the game has allocated certain vertical or horizontal space for separate dimensions that are teleported to in gameplay.

The world boundary is an invisible barrier which isn't supposed to be passed by normal means. Beyond the world boundaries, no node can be placed and there is just an endless void technically occupied by the Ignore node. Luanti's world is a huge cube with a side length of ca. 60000 nodes and the dimensions (X, Y and Z) ranges from −30912 to 30927 by default. The theoretical boundaries are at the signed 16-bit integer limit, but have been cut short in order to reduce bugs that may be exhibited at the theoretical boundary.

You can view your current position by pressing the F5 (default Debug key) on your keyboard, but this debug information can be disabled by games using the `basic_debug` HUD flag. In which case they may have other means to determine your position in the world.

## Changing the world boundary

If you want to limit the world boundary to be smaller than the default then this is possible by using the `mapgen_limit` map setting (stored in `map_meta.txt` for existing worlds). This will be rounded to the nearest mapchunk and will only apply to the generation of new mapblocks however, so if there are existing mapblocks generated outside the limit then these will still show up. The setting also controls the limit in every dimension equally, and there is no way to have more control over the boundary.

If you wish to have more control over making a limited world size you will need to implement this in Lua, to override the mapgen and replace all nodes beyond a given coordinate with an invisible barrier node.
