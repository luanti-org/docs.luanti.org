---
title: FAQ
aliases:
  - /FAQ
  - /faq
---

# FAQ

This is a collection of some **frequently asked questions** about Luanti. For technical problems, refer to [Troubleshooting](/for-players/troubleshooting).

## What is Luanti?

Luanti is a free software game engine designed to create [voxel](https://en.wikipedia.org/wiki/Voxel)\-based games. A detailed description can be found at [Luanti](/about/luanti).

## About the Engine

### How much does Luanti cost?

Nothing. It's free.

### Is Luanti a clone of Minecraft?

No. Luanti has very different goals from Minecraft, and doesn't aim to compete with or replace Minecraft. Luanti is an engine and a platform for many different voxel games, rather than one cohesive gaming experience.

When Luanti was initially created in 2010 it intended to replicate what Minecraft Alpha had been shown to do at the time, but it has later diverged into becoming more akin to a game engine.

### Who created Luanti?

Luanti was originally created by [Perttu "celeron55" Ahola](https://www.c55.me/). The engine is currently developed by [this random bunch of lunatics](https://github.com/orgs/minetest/people) as well as [the community](https://github.com/luanti-org/luanti/graphs/contributors).

### What is Luanti written in?

The Luanti engine is primarily written in C++ using a [forked version of the Irrlicht rendering engine](#what-is-irrlicht). Some parts of the engine are implemented in the [Lua language](https://www.lua.org/), and the scripting API uses Lua as well.

### Can I change the code myself?

[Yes.](https://github.com/luanti-org/luanti/) Luanti is freely licensed ([LGPL 2.1 and others](https://github.com/luanti-org/luanti/blob/master/LICENSE.txt)).

### When will the next version of Luanti be released?

Check the [GitHub milestones](https://github.com/luanti-org/luanti/milestones) for a good idea of how far along the next version is.

#### Why did the version change from 0.4.17.1 to 5.0.0?

Luanti switched to [semver](https://semver.org/) after 0.4.17.1 because the leading zero was never going to change. In the old versioning system 5.0.0 would be 0.5.0.0. Semver is cleaner and is an industry standard.

### How do I install and run Luanti?

See [Getting Started](/for-players/getting-started/).

### How do I update Luanti?

This depends on your operating system and installation method. You will usually either download the newest version or run an update command. Some systems may require you to move your worlds and mods. If you are using the Git version, simply pull and compile every time you want to update.

### Where and how do I find games to install?

When you boot up Luanti for the first time, it will point you to the content browser to install a game. Afterwards you can find it by going to the Content tab in the main menu. The selection of games is also available to browse in your browser on [ContentDB](https://content.luanti.org/packages/?type=game).

### What is Irrlicht?

Irrlicht is a rather ancient real-time 3D rendering library. Currently Luanti uses it for rendering, managing the window, the underlying GUI, input handling, model and image format support.

As it's old and mostly unmaintained, there is work on moving away from Irrlicht. Currently Luanti uses its own fork of Irrlicht called [IrrlichtMt](https://github.com/luanti-org/luanti/tree/master/irr#irrlichtmt-version-19), which is supposed to distill Irrlicht down to what Luanti actually uses, along with fixing issues that were previously out of reach. The goal is to reduce the reliance on Irrlicht in favour of SDL2 and our own rendering pipeline.

### Why does the UI look so old?

The main menu and Minetest Game more or less uses the default [Irrlicht](#What_is_Irrlicht.3F) styling from before [formspec styling](https://api.luanti.org/formspec/#styling-formspecs) was introduced.

It is certainly possible to improve the UI, and some games style their GUI wonderfully, but deciding on changes to the engine's default styling that everyone can agree on is difficult. Contributions are welcome.

### The main menu sucks!

[We're working on it.](https://github.com/luanti-org/luanti/issues/6733)

### Is Luanti multi-threaded?

Yes. Luanti's client and server run in different threads. The server also runs map generation in worker threads, although this is limited to a single thread currently due to issues (but [can be changed in settings](https://github.com/luanti-org/luanti/blob/eea2a97/minetest.conf.example#L2973-L2984)). The client runs mesh generation in worker threads. As of 5.7.0, mapblock rendering is done in parallel on several threads.

It's rare for games to be heavily concurrent as it's not needed for smaller games - it's usually only found in MMOs. It's a lot harder to write multi-threaded gameplay logic, which is in conflict with Luanti's aim to be an easy-to-use content creation platform. There is an API for mods to run tasks in a worker thread, however.

### Why is it called "Luanti"?

The name "Luanti" is a play on words from the Finnish word "Luonti" meaning creation, and the Lua programming language. The name change was first announced on the 13th of October 2024 and came into effect with the 5.10 update. [The blog post announcing the name change](https://blog.luanti.org/2024/10/13/Introducing-Our-New-Name/) goes over the motivation behind it in more detail.

### What is "Minetest"?

It is the old name for Luanti, which is slowly being replaced with the new name but will realistically never fully disappear.

The origin of the old name was due to it starting as an experiment (a **test**, if you will) to replicate **Mine**craft Alpha all the way back in 2010. The name stuck, and [no one could agree](https://forum.luanti.org/viewtopic.php?f=5&t=17133) [on a new one](https://github.com/luanti-org/luanti/issues/13510). [Until we actually did](https://blog.luanti.org/2024/10/13/Introducing-Our-New-Name/).

### Can you add support for iOS?

We are unable to add Luanti to the App Store as there is a license conflict. The App Store's EULA contains terms that limit software freedom, which (as far as the consensus goes) conflicts with (L)GPL licenses.

Whilst it theoretically would be possible to distribute apps outside of the App Store, iOS is generally anti software freedom. Users would need to either pay $80 a year for a developer license and have a Mac Book, or jailbreak their phone. Neither of these things are particularly desirable. **Edit**: looks like there are now third-party ways to sideload, and the EU may be forcing Apple to allow sideloading in the future, so this may not apply.

Another reason is that we have limited time, and we do not have a core developer that could be responsible for the development of an iOS version. See the question below.

### Can you add support for X platform or console?

Luanti is a volunteer project with limited time and resources. Supporting a new platform takes a lot of effort, and we're only able to officially support platforms with a core developer using it.

In terms of consoles, if someone is willing to make a homebrew port for a game console then it can be done, and this port can be submitted to upstream given that the changes aren't intrusive and some kind of maintenance is kept. Putting Luanti onto a games console in any official capacity is not possible due to NDAs preventing an open-source release, which is incompatible with the LGPLv2.1 license.

Luanti is open-source. We welcome contributors to port Luanti to other platforms (providing they follow the licenses of course). Contributors are also welcome to contribute their changes back upstream as well, providing that they're not too intrusive.

### Where can I ask questions?

See [Getting involved](https://www.luanti.org/get-involved/) for links to all Luanti community spaces where you can ask questions. It is recommended to search history and archives first, many questions have already been answered.

Be mindful of which category (thread, channel, etc.) your question may fall under when asking on certain platforms.

### Are mods compatible with each other?

Unless otherwise noted, most mods will just work with each other (they may or may not interact perfectly). Different API implementations (such as mobs or stairs), game-specific mods, or competing content implementations may not be compatible with each other.

### How do I connect to servers? Do I need a multiplayer account?

There is no globally valid multiplayer account in Luanti. For each server, you can use a different name and password. The "account" is created at first login. This way, only you can access your player and inventory on that particular server. You can always play singleplayer with no need for any username and password.

### How do I make my own server?

See [Setting up a server](/setting-up-a-server/).

### How do I install third-party content?

You can find all kinds of community-made content on [the Luanti ContentDB](https://content.luanti.org/). You can find and install content from this database using "Browse online content" found in the **Content** tab in the main menu. See [How to install content](https://content.luanti.org/help/installing/).

## Terminology

For a complete list of Luanti terminology, see [Glossary](/about/glossary/).

## About Minetest Game

### What is Minetest Game?

Minetest Game is a game for the Luanti engine. It is simply one game like any other, previously it used to come bundled with the engine but is now available for download on ContentDB.

In its vanilla state, it might be considered too boring of a game. It can be enough if you want to build in creative, but you will need to install additional mods for it to become a decent survival gameplay experience.

### Where is \[insert Minecraft feature here\]?

Minetest Game is not intended to replicate Minecraft in any way. Features from Minecraft may be found in mods or other games (try out [Mineclonia](https://content.luanti.org/packages/ryvnf/mineclonia/) for a close analog of Minecraft). However if you would like to play a game with Minecraft features, we recommend just playing Minecraft.

#### Which Mineclone should I play?

[Voxelibre](https://content.luanti.org/packages/Wuzzy/mineclone2/) used to be called MineClone 2, but has changed its focus with the rebrand to no longer be a Minecraft clone.

[Mineclonia](https://content.luanti.org/packages/ryvnf/mineclonia/) is a fork of Voxelibre (when it was still called MineClone 2) that is more focused on stability and performance. They merge fixes from Voxelibre, but keep the original goal of being a Minecraft clone.

MineClone 5 used to be a fork of MineClone 2 without a version milestone, back when MineClone 2 was frozen at 1.12. This is now no longer the case, and MineClone 5 is abandoned.

#### What is MeseCraft?

Contrary to what the name may suggest, [MeseCraft](https://content.luanti.org/packages/MeseCraft/mesecraft/) is not a Minecraft clone. It is a collection of mods intended to provide a curated, ready-to-go survival experience.

### Where is all the content? No mobs? No story?

Minetest Game is not designed with a story or goal in mind. It is simply a sandbox to play in. Mods or other games provide actual gameplay content. Try looking for some on the [ContentDB](https://content.luanti.org/).

### You should add \[insert feature here\] to Minetest Game!

Minetest Game is [currently in maintenance-only mode](https://github.com/luanti-org/minetest_game/issues/2710). New gameplay features are not being considered and should be added yourself as a mod.

### How does \[insert item or block here\] work? How do I craft it?

Refer to [nodes](/for-players/nodes) and [items](/for-players/items) for information on usage and crafting. You can also install a crafting guide mod to view available crafting recipes.

### I was told there were dungeons. Where are they?

Make sure you enable dungeons when you create your world. They tend to spawn generally anywhere under or on the surface, you'll run into one eventually.

### Why can't I find the mod `default` or \[insert mod here\]?

Some mods are part of games. Mods like `default` and `wool` are usually part of Minetest Game and are not meant to be installed as separate mods. For a mod depending on e.g. `default` to work, you will need to use Minetest Game. Good game design practice is to prefix game-specific mods with the game name. Minetest Game is a particularly confusing case, unfortunately.

## Gameplay

### How do I see my coordinates or check my FPS?

Use the F5 key to toggle debug info. The top of your screen may display your FPS and coordinates, among other information. If the game or server you're playing on disables this information then it won't be visible. Pressing F5 again will toggle the profiler, and then wireframe mode if the **debug** [privilege](/for-players/privileges) is possessed. A final press will toggle debug info off.

### Why is my FPS so low?

There are many reasons your FPS could be low. A common culprit is having infinite viewing range enabled. This can be toggled, but there is no keybind set to do so. You can also try decreasing your regular view range using the \- by default (+ to increase). Render distance is measured in nodes.

### How do I enable full-screen?

On many systems your OS fullscreen key (such as alt+F11 on Ubuntu) should work just fine. Otherwise, navigate to **Advanced Settings** in the **Settings** tab in the main menu, and go to _Graphics > In-Game > Advanced > Fullscreen_, where you can double-click to change the value or use the **Edit** button. You will need to restart Luanti for this to take effect.

### What are the controls? How do I change them?

_See [Controls](/for-players/controls)._

### Can I use a controller or gamepad?

Yes. It is recommended to use an external program to bind them, as the current engine implementation of them functions poorly. See [Gamepads](/for-players/gamepads) for more info.

### Why can't I change settings in the pause screen?

Many settings require the game to reload anyway, but otherwise there simply isn't a menu implemented to do it. [Contributions welcome](https://github.com/luanti-org/luanti/pulls).

### How big is the map?

The map is a cube with a side length of 61840 nodes. The map has thus a volume equal to the volume of 618403 nodes = 236,487,637,504,000 nodes. The coordinates range from −30912 to 30927 in all dimensions. See also [World boundaries](/for-players/world-boundaries/)

### The map is too small! Can it be expanded?

Not currently. The current size of the world is plenty large, in addition to being approximately 62000 nodes in horizontal size, the world is also approximately 62000 nodes tall giving a lot of vertical space to expand to.

The reason the map is limited to this is due to the integer types of positions, along with float precision becoming less accurate the further from the origin one could theoretically go. The fix would require a lot of changes to the code, and cause a multiplayer netcode compatibility break which isn't something desirable right now and is of a lesser priority compared to other issues with the engine.

### Can I change my skin?

Skins are implemented per-game and per-world, as there is no central authentication server where skins are managed. If you'd like to change your skin in singleplayer, try using one of the [many skin mods](https://content.luanti.org/packages/?type=mod&q=skin). To change your skin on a server, use their skin mod (if any) or contact a member of management.

#### What format do skins use?

Minetest Game uses pre-1.8 Minecraft skins (64x32). These support regular body texture, a head overlay, 4px-wide arms, and a cape. Other skin mods implement 64x64 skin support or other formats.

### How do I set my spawn point?

If you play Minetest Game, you can build a bed and sleep at night. On your next life, you will spawn on the bed.

Otherwise, you set your spawn point for _all_ worlds using `static_spawnpoint = (x,y,z)` in your [`minetest.conf`](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example) file.

### How do I make the world flat?

Use the `flat` mapgen when creating your world and disable all the mapgen flags. If you want the world to use a single material, try the [Really Flat mod](https://content.luanti.org/packages/rdococ/really_flat/).

### Can I add fancy shaders?

As of 5.6.0, Luanti comes with toggle-able dynamic shadows. Game support is required, if your game in particular does not support it you may use the `[enable_shadows](https://content.luanti.org/packages/ROllerozxa/enable_shadows/)` mod. More advanced post-processing shaders, such as bloom or volumetric lighting, are in 5.7.0.

### How do I increase the brightness?

_See [Troubleshooting#The_screen_is_too_dark](/for-players/troubleshooting/#the-screen-is-too-dark)._

### How do I fly?

To fly, you first need the "fly" [privilege](/for-players/privileges). Use the K key to toggle flying. Ascend with the jump key (default: space bar) and descend with the sneak key (default: shift).

See also: [Controls](/for-players/controls)

### How do I sprint?

With default Luanti behavior you can "go fast" using the "fast" [privilege](/for-players/privileges) (toggled with the J key) and your auxiliary key (default: E). This is a lot faster than natural sprinting. There are [some mods](https://content.luanti.org/packages/?type=mod&q=sprint) and games that implement conventional sprinting.

See also: [Controls](/for-players/controls).

### How do I find my house again?

You can keep track of your coordinates using debug info (F5), if the game lets you see this information. If you want, you can teleport to coordinates using `/teleport x y z`

_See [Server commands#Teleportation](/for-players/server-commands/#teleportation). Requires "teleport" [privilege](/for-players/privileges)_.

In Minetest Game, you can avoid getting lost again if use `/sethome` at home to save your home position and `/home` to teleport back to it.

### How do I generate a map of my world?

To generate a top-down image of your world you can use [`minetestmapper`](https://github.com/luanti-org/luantimapper), and to generate an isometric view of a part of the world you can use [`maprenderer`](https://github.com/minetest-go/maprenderer).

### Can I use WorldPainter with Luanti?

The author of WorldPainter has created a [Luanti plugin](https://www.worldpainter.net/files/plugins/LuantiSupport.jar) for WorldPainter, and updates of the plugin are posted in [this Luanti forum thread](https://forum.luanti.org/viewtopic.php?t=16649). Your mileage may vary.

### Is there a WorldEdit for Luanti?

[Yes.](https://content.luanti.org/packages/sfan5/worldedit/) There are also other world editing mods available on [ContentDB](https://content.luanti.org/packages/?tag=building_mechanics).

### Can I convert Minecraft worlds and content to Luanti?

Yes, you can use [MC2MT](https://github.com/rollerozxa/MC2MT) to convert Minecraft worlds up to 1.12 into Mineclonia. Your mileage may vary.

## Game and Mod Development

### What is a mod?

A mod (modification, alternatively can be interchanged with module in the case of Luanti) is a collection of code that can implement or change a feature. The simplest mod consists of just an `init.lua` inside a folder in the mods/ directory, and makes calls to Luanti Lua modding API in order to implement gameplay features or do other cool things. "Games" in Luanti consist of a collection of mods accessible from the main menu, more akin to "modpacks" in another voxel game.

### What language are games and mods written in?

Luanti uses the [Lua language](https://www.lua.org/) for games and mods. It is simple, light, and fast. You can find plenty of tutorials online.

- [Programming in Lua Book](http://www.lua.org/pil/contents.html), learn the Lua language (for Lua 5.0)
- [Lua 5.1 reference](http://www.lua.org/manual/5.1/) (note that Luanti uses Lua 5.1)
- [Lua guide by tutorialspoint](https://www.tutorialspoint.com/lua/lua_overview.htm)
- [Interactive Lua course by Coddy](https://coddy.tech/landing/lua), write and run Lua in the browser
- [Lua in 100 Seconds (Video)](https://www.youtube.com/watch?v=jUuqBZwwkQw)
- [Lua in 15 Minutes for Programmers (Video)](https://www.youtube.com/watch?v=kgiEF1frHQ8)

### How can I learn to make games and mods?

We recommend the [Luanti Modding Book](https://rubenwardy.com/minetest_modding_book/en/index.html) to start. For more experienced users there is the [Lua API reference](https://github.com/luanti-org/luanti/blob/master/doc/lua_api.md) available in the Luanti source tree, which is also available [in a pretty HTML version](https://api.luanti.org/).

If you need further help feel free to ask any questions in the forums or in any of the discussion channels (e.g. IRC, Discord...).

#### How fast is Lua?

[Really](https://programming-language-benchmarks.vercel.app/lua-vs-java), [really fast](https://programming-language-benchmarks.vercel.app/lua-vs-cpp).

#### Why can't I use modern Lua features?

Luanti uses [LuaJIT](https://luajit.org/) which is based on Lua 5.1. In the future if PUC Lua ever matches JIT performance, Luanti could change to a newer version of Lua.

#### Can I write games or mods in \[insert language here\]?

If it can transpile to Lua or you connect your own interpreter, yes. E.g. [Loria](https://content.luanti.org/packages/siegment/loria/) is written in [Fennel](https://fennel-lang.org/). Otherwise, support for other languages will not be made first-class. Lua is designed for embedding and we will stick to it.

#### I want to use this Java mod! I need Java!

Lua can easily outperform a bridge to Java in most cases. If you really need to use another language for something like heavy data crunching, Lua can natively call C/++ libraries (only usable if a mod is [trusted](#What_is_mod_security.3F)).

### What is the Luanti API?

The Luanti API gives you access to everything the engine currently has to offer. You can find the latest API reference [here](https://github.com/luanti-org/luanti/blob/master/doc/lua_api.md). A ReadTheDocs version can be read [here](https://api.luanti.org/).

### Can mods overwrite the engine?

Luanti mods must use the engine-provided API. Unlike Minecraft, Luanti mods cannot change the engine. If you think the engine is missing a feature, consider [opening an issue](https://github.com/luanti-org/luanti/issues) or [write the feature yourself](https://github.com/luanti-org/luanti/pulls).

### What is a formspec?

Formspec is the name of Luanti's UI toolkit, and is used to make dialogs and forms for mods. For more information see the [Lua API documentation](https://minetest.gitlab.io/minetest/formspec/#formspec).

### What is a listring?

Listrings are a convenience feature in Luanti to allow quick transfer of an itemstack from one inventory list to another with `Shift+Click`. Basically, listrings group several inventory lists together in a “ring” data structure. A shift-click will transfer an itemstack from one inventory list to the next inventory list in the listrings, and, if it is the final inventory list in the listring, it will be sent to the first inventory list. ([see Lua API documentation](https://minetest.gitlab.io/minetest/formspec/#listringinventory-locationlist-name))

### How do I do biomes?

Biomes are distributed according to their heat and humidity values. These values generally range between 0 and 100. A voronoi diagram can be used to visualise the distribution, [see the modding book](https://rubenwardy.com/minetest_modding_book/en/advmap/biomesdeco.html#biome-placement).

### What is devtest?

Development Test (devtest) is a game for testing engine features and doing mod development. It provides a minimal set of game content along with an extensive library of content for testing engine features. It is available in the source tree, or can be downloaded from ContentDB.

### What is mod security?

Mod security prevents mods from executing insecure methods without user approval. A mod needs to be explicitly specified as HTTP to utilize the HTTP API. A mod needs to be trusted to run many OS-level methods.

### How do I debug my Lua code?

The "lua" interpreter is not very useful for debugging mod code, because mods are built to run in an environment specific to Luanti (they expect to be loaded after their dependencies, have access to the Luanti API, etc.). However, you may be able to break pieces of your logic into separate, stand-alone Lua library files that are not specific to Luanti. Where you have an opportunity to do this, testing outside of Luanti (using Lua 5.1's "lua" interpreter or LuaJIT's "luajit" interpreter) can be helpful. This might work for some utility functions and "class libraries".

There is no built-in debugger in Luanti, apart from "printf-style debugging" with functions like `minetest.log()` and `dump()`. There exists mods such as [dbg](https://content.luanti.org/packages/LMD/dbg/) which gives more advanced debugging capabilities than are available by default.

### I get an error or warning message, what does it mean?

_See [Troubleshooting#Error messages without crashes](/for-players/troubleshooting/#error-messages-without-crashes)._

### Why can't I make other keybinds?

This is a missing feature. [Contributions welcome](https://github.com/luanti-org/luanti/pulls).

### How do I make models for my mods?

Luanti supports OBJ for static models, and both static and animated GLTF models are supported starting with 5.10. You can use Blender, Blockbench or any other 3D modelling program that supports these formats.

#### Can I use Blockbench to make models?

Yes, you can make both static and animated models using Blockbench now that Luanti supports GLTF models in 5.10. See [Using Blockbench](/using-blockbench/) for more information.

### Can I distribute games outside of Luanti?

Yes, but this is not officially supported in the engine, you will need to maintain a re-branded fork of the engine. See [Distributing Games Outside ContentDB](/for-creators/distributing-games-outside-contentdb.md/) for notes on how to do so.

### Can I sell games I make with Luanti?

As long as you comply with [the license](https://github.com/luanti-org/luanti/blob/master/LICENSE.txt), yes. For the engine, you will need to publish the source code to any modifications you've made to it. If you use others' mods in the game then make sure you comply with their license terms and that they don't contain non-free assets that prevent commercial usage.
