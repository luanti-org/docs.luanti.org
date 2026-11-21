---
title: Custom Lua Mapgen
aliases:
  - /mapgen/custom-lua-mapgen/
---

The Luanti engine provides several built-in map generators implemented in C++, where biomes and decorations can be registered by mods. In addition to this you can also write your own mapgen in Lua. This is done by hooking into the `singlenode` mapgen (by default consisting only of `air`) and generating your own terrain using the Lua API.

## Performance

As of 5.9, it is possible to write Lua map generators that run in a separate environment for each mapgen thread (see [Mapgen environment](https://api.luanti.org/core-namespace-reference/#mapgen-environment)), improving the performance of the mapgen.

Previously custom Lua mapgen would be implemented with the `on_generated` callback which would run on the main Lua thread, which is not recommended anymore unless you cannot implement something due to the limitations of the isolated mapgen environment.

Even with the new mapgen environment you should still be wary of some pitfalls that can hinder the performance of your Lua mapgen. For a list of optimization techniques you can use to write performant mapgens, see [Mapgen memory optimizations](/for-creators/memory-optimizations/).

## Examples

A good initial example of how to write a custom Lua mapgen is [lvm_example](https://content.luanti.org/packages/ROllerozxa/lvm_example/). It incorporates most optimizations techniques to create a performant mapgen and uses Perlin noise to generate random looking terrain, but has not been updated yet to use the new async mapgen environment.

For more examples of custom Lua mapgens, see the [Custom mapgen](https://content.luanti.org/packages/?type=mod&page=1&tag=custom_mapgen) tag on ContentDB.

## Libraries

- [Luamap](https://content.luanti.org/packages/MisterE/luamap/) is a library that seeks to take out some of the pain of making custom mapgens by abstracting it away.
- [Biomegen](https://content.luanti.org/packages/Gael%20de%20Sailly/biomegen/) reimplements the biome system used in built-in C++ mapgens for use with custom Lua mapgens.
