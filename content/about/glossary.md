---
title: Glossary
aliases:
  - /Terminology
  - /glossary
---

This page contains a hopefully complete list of terminology related to Luanti.

## Luanti and its components

- **Luanti**: The game engine whose wiki you are on right now, generally refers to the engine.
- **Minetest**: The former name of Luanti
- **IrrlichtMt** / **Irrlicht**[^1]: The name of Luanti's 3D rendering library. It was forked by the Luanti developers from the original [Irrlicht](https://irrlicht.sourceforge.io/), which is ancient and poorly maintained nowadays.
  - IrrlichtMt has been merged into the Luanti repository as the [`irr/`](https://github.com/luanti-org/luanti/tree/master/irr) directory.
- **Game**: A collection of mods and some additional metadata, which can be selected from the main menu. Not to be confused with Modpack which is a pack of mods selectable like regular mods.
  - Previously were referred to as **Subgames**, but this term is outdated. Please update any references to it as such.
- [**Minetest Game**](https://content.luanti.org/packages/Minetest/minetest_game/) (abbreviated MTG): The name of the game that used to be shipped with Luanti by default.
  - [minetest_game](https://github.com/luanti-org/minetest_game): Repository name of Minetest Game and technical name of the game.
- [**Mods**](/for-players/mods) : What all games consist of, or can be added on top of an existing game. They are written in Lua using Luanti's modding API.
  - Sometimes abbreviated SSM, Server-Side Mod to make the distinction that regular Luanti mods run on the server as compared to CSMs which run on the client.
- **Client-Side Mod** (abbreviated **CSM**): An experimental API allowing for the client itself to run Lua code.
  - **Server-Sent Client-Side Mod** (abbreviated **SSCSM**): A currently unimplemented feature referring to CSMs being sent from the server to the client, to allow for code to be run on the client for improved latency, among other things.
- **Modpack**: A collection of mods which can be selected like a regular mod. Not to be confused with a Game, which can be selected from the main menu as its own distinct game.
- **Texture pack**: A collection of textures that can replace the textures in games and mods. May only support specific games and/or mods.
- **[ContentDB](https://content.luanti.org/)** (abbreviated CDB): The official place to upload and download games, mods and texture packs. It is what powers the content browser accessible from the main menu.
- **Server**: The program that manages and distributes mod, texture and sound data to the players. It also does all of the mod initiation.
- **Client**: The program that the player uses to connect to singleplayer or multiplayer games. Handles the rendering of the world.
  - In singleplayer, the client will start a server in the background restricted to the _singleplayer_ user which will then join it, acting both as a client and a server in that case.
- **[minetest.conf](/for-players/minetest-conf)**: A file in the root directory of Luanti's user directory which contains configuration settings for Luanti. A list of the settings can be found in the main menu's "All settings" menu or in [**minetest.conf.example**](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example).

## In-game Content

- **World**: A saved game in Luanti, which contains:
  - **Map**: a [database](/for-server-hosts/database-backends) of the map's content, everything from blocks to chest contents.
  - Player passwords, their health, and their inventory.
  - Other needed files, such as ban lists, seed numbers and protected areas.
- **Nodes**: 1×1×1 meter individual cubes in the game, and are grouped and loaded by their mapblocks. Informally referred to as blocks by regular players.
  - **Node Metadata**: Additional data that is attached to a node's position in the map. For example container nodes use this to store their inventories, and signs use this to store what is written on them.
  - **Liquids**: Nodes with _some_ kind of property related to liquids. In development, this term is problematic when used without any clarification, as node properties related to liquids are decoupled. To avoid confusion, say to which _precise_ technical property you refer to, for example, the "liquid" drawtype, liquid spreading/flowing, liquid viscosity, liquid player movement physics, etc.
- **Blocks**/**MapBlocks**: 16×16×16 groups of nodes.
- **Sectors**: Stacks of single mapblocks, extending from the absolute bottom of the map to the top.
- **MapChunks**: Groups of 80x80x80 nodes (5x5x5 mapblocks), they are an abstraction used only for mapgen for performance reasons.
- **Object**: A movable thing that is not bound to a fixed position on the map. Following kinds of objects exist:
  - **Entity**: An object that is defined and controlled by Lua. For example: mobs, falling nodes or primed TNT.
  - **Player**: An object that represents a player.
- **Items**: Any thing that can be put inside of an inventory. Items _can_ have special uses like digging, dealing damage, crafting, placing, etc., but this is optional. Note the 3 item types are a technical distinction, not a semantic one.
  - **Tool**/**Tool item**: a type of item that is non-stackable and has a "wear" property so it is capable of wearing out and break.
  - **Node item**: the "item form" of a node which can be placed by default. Every node has a node item associated with it. For example, cobblestone from Minetest Game is a _node_ when it exists in the world, but it is a _node item_ when in an inventory.
  - **Craftitem**: A type of item that is neither a tool nor a node item.
- **Item stacks**/**ItemStack**: Stacks of identical items with an item count.
- **Environment**: A global Lua/C++ scripting object that (amongst other things) provides access to all nodes, (active) objects and players.
- **Active Block Modifier** (Abbreviated ABM): Functions that run on nodes of a certain type, changing them or giving them interactive properties (eg. grass and moss growth).
- **Loading Block Modifiers** (Abbreviated LBM): Similar to ABMs but are only run when a mapblock is loaded. They can also only run once, depending on its definition.

## Lua and Modding-related

- **Lua**: A simple, minimal and fast programming language which is used for Luanti's API.
- **Modding API**/**Lua API**/**Luanti API**: a selection of functions and values, used by mod files to modify, extend or add features and blocks. API stands for Application Programming Interface.
- [**lua_api.md**](https://github.com/luanti-org/luanti/blob/master/doc/lua_api.md): A file in the `doc/` directory that contains the full reference of the Lua API. The same content is also available in [HTML format](https://api.luanti.org/).

## Notes

[^1]: Officially, Luanti's rendering library is called "IrrlichtMt", but the name "Irrlicht" is used interchangeably. When we say "Irrlicht", we usually mean Luanti's rendering library, not the original one
