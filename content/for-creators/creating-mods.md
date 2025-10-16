---
title: Creating mods
---

# Creating mods

Luanti has a scripting API which is used to program games and [mods](/for-players/mods), allowing creators to create new experiences or extend existing ones with mods. The API is accessed using [Lua](https://www.lua.org/), an easy-to-use programming language. Version 5.1 of Lua is used, but many people run LuaJIT for greater performance. The only thing you will need is _basic_ programming knowledge.

The official Luanti Lua API reference can be found at [api.luanti.org](https://api.luanti.org/), with guides hosted in the [API section of this site](/for-creators/api).

## Modding tutorials

The best way to create your first Luanti mod or game is with the [Luanti Modding Book](https://rubenwardy.com/minetest_modding_book/en/index.html) by [rubenwardy](https://rubenwardy.com/) with editing by [Shara](https://forum.luanti.org/memberlist.php?mode=viewprofile&u=19807).

Games use the same system as mods, and can be thought of as simply a collection of mods. See the [Games chapter of the Luanti Modding Book](https://rubenwardy.com/minetest_modding_book/en/games/games.html) for more details.

## Recommended tools

Here are some useful tools that most modders use when making Luanti mods:

- [Visual Studio Code](https://code.visualstudio.com/)/[VSCodium](https://vscodium.com/), a powerful code editor. We've also published Minetest Tools, an extension for Luanti code completion available on [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=GreenXenith.minetest-tools) and [Open VSX Registry](https://open-vsx.org/extension/GreenXenith/minetest-tools).
- [luacheck](https://github.com/lunarmodules/luacheck), a static analysis tool for Lua. See the [luacheck chapter of the Luanti Modding Book](https://rubenwardy.com/minetest_modding_book/en/quality/luacheck.html) for more information on how to use it with Luanti.
- [Lua Language Server](https://luals.github.io/) with type documentation by [Archie](https://git.minetest.land/archie/luanti-api/)

## See also

- [Modding Tips on this site](/for-creators/modding-tips)
