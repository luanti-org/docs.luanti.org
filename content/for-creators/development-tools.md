---
title: Development Tools
aliases:
  - /Development_Tools
  - /development-tools
  - /content-dev/development-tools
---

# Development Tools

This page contains a list of tools that are useful for developing games and mods for Luanti, or working with parts of the engine.

In addition to this list, you can also see the [Development Tools](https://content.luanti.org/packages/?tag=developer_tools) tag on ContentDB for mods useful for development and the [modtools](https://github.com/luanti-org/modtools) repository for officially maintained modding tools.

## Engine development

- Development Test: A testing game for testing various engine features
- [/luanti/util](https://github.com/luanti-org/luanti/tree/master/util): Various maintenance utilities

## General Lua Tools

- [luacheck](https://github.com/lunarmodules/luacheck): Lua linter and static code analyzer (see also [the chapter in rubenwardy's modding book](https://rubenwardy.com/minetest_modding_book/en/quality/luacheck.html))
- [busted](https://olivinelabs.com/busted/): Lua unit testing framework (see also [the chapter in rubenwardy's modding book](https://rubenwardy.com/minetest_modding_book/en/quality/unit_testing.html))
- [Lua pattern viewer](https://gitspartv.github.io/lua-patterns/): A site like [Regexr](https://regexr.com/) but for visualizing and working with Lua patterns.
- [Warr1024's Lua Code Tools](https://gitlab.com/Warr1024/luatools): Lua formatter & locals generator

## Debugging/profiling

- [debug](https://content.luanti.org/packages/LMD/dbg/): Luanti mod library that offers more debugging capabilities.
- [LuaJIT Profiler](https://content.luanti.org/packages/jwmhjwmh/jitprofiler/): Luanti mod that allows you to profile mods using LuaJIT's built-in profiler and flame graph.
- [Warr1024's Minetest Server Docker Image](https://gitlab.com/sztest/minetestserver): Comes with useful debugging and performance profiling tools

## Syntax highlighting/autocompletion

Note: the extensions below are community projects, not maintained by the Luanti team.

- **"Luanti API"** extension:
  - Features:
    - Typed Luanti API: classes (ObjectRef, InvRef, ItemStack, Settings, ...), `core.*` functions, and definition tables are described with exact types
    - Context-aware autocompletion: after `player:` only player methods are suggested, after `inv:` only inventory methods, inside a node definition only its fields and callbacks
    - API documentation shown in a popup as you type and on hover
    - Diagnostics: warns about typos, wrong arguments and wrong types before you run the game
    - Built on top of [EmmyLua](https://marketplace.visualstudio.com/items?itemName=tangzx.emmylua) Language Server (installed automatically), which also provides:
      - Autocompletion for your own functions, methods and tables, across all mods in the workspace
      - Type annotations for your own code (`---@param`, `---@return`, `---@class`, ...) using [LuaCATS](https://luals.github.io/wiki/annotations/) annotations
    - Screencast with examples: see the extension page (links below)
    - Not included: mod and game folder templates, `.luacheckrc` generator, formspec syntax highlighting
  - Get it at:
    - [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=Voxrame.luanti-ide-helper) (VS Code)
    - [Open VSX Registry](https://open-vsx.org/extension/Voxrame/luanti-ide-helper) (VSCodium, Cursor, Windsurf, Theia and other VS Code forks)
  - Other editors: [installation instructions](https://github.com/Voxrame/luanti-ide-helper#supported-ides--plugins)
- **"Luanti Tools"** extension:
  - Features:
    - Lightweight snippet-based autocompletion for `core.*` functions and object methods, with a few simplifications:
      - method suggestions after `:` are not tied to the object type, so methods of all classes are listed
      - fields and callbacks of definition tables are not suggested
    - Short description for each suggestion with a link to the relevant section of `lua_api.md`
    - Formspec syntax highlighting and texture modifier suggestions
    - Commands to create mod and game folder templates
    - `.luacheckrc` generator
  - Get it at:
    - [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=GreenXenith.minetest-tools) (VS Code)
    - [Open VSX Registry](https://open-vsx.org/extension/GreenXenith/minetest-tools/) (VSCodium, Cursor, Windsurf, Theia and other VS Code forks)

## Schematics

- [Schematic Editor](https://content.luanti.org/packages/Wuzzy/schemedit/): Luanti mod that allows you to import, edit and export schematics straight within Luanti.
- [MTS Editor](https://forum.luanti.org/viewtopic.php?f=14&t=23724): Standalone program to view and edit Luanti schematics, but it supports other file formats (e.g. Minecraft schematics) as well.
- [Lua2Mts](https://content.luanti.org/packages/Neuromancer/lua2mts/): Luanti mod to convert .lua schematics to .mts schematics.
- [ASCII Art Schematics](https://content.luanti.org/packages/Warr1024/aaschems/): Library to manage Luanti schematics as ASCII art

## Biomes

- [Luanti Biome Point Visualizer](https://wuzzy.codeberg.page/LiBPoV/): Edit and visualize biome heat/humidity points in a Voronoi diagram ([source code](https://codeberg.org/Wuzzy/LiBPoV))

## Perlin Noise

- [Perlin Explorer](https://content.luanti.org/packages/Wuzzy/perlin_explorer/): Luanti mod that allows you to experiment with all sorts of Perlin noise.
- [Perlin noise tuner](https://codepen.io/treer/pen/gOPZyov?editors=0010): Visualizes 2D Perlin noise that Luanti will generate with different noise params.(Emulation of Luanti Perlin noise can be wrong in extremes/edge-cases due to precision of JavaScript number type)

## 3D models

- [Blender](https://www.blender.org/): Essential for making animated models in Luanti, see [Using Blender](/for-creators/models/using-blender/).
- [Blockbench](https://www.blockbench.net/): Useful for making static voxel models for Luanti, see [Using Blockbench](/for-creators/models/using-blockbench/).

## Images

- [optipng](https://optipng.sourceforge.net/): Lossless PNG compressor
- [pngcrush](https://pmt.sourceforge.io/pngcrush/): Lossless PNG compressor
- [pngquant](https://pngquant.org/): _Lossy_ PNG compressor
- [guetzli](https://github.com/google/guetzli): JPG compressor

## Formspecs

- [formspec_editor](https://content.luanti.org/packages/Just_Visiting/formspec_editor/): Luanti game that allows you to write formspecs and preview them instantly within Luanti
- [Luanti Formspec Editor](https://luk3yx.gitlab.io/minetest-formspec-editor/): WYSIWYG online formspec editor

## Translation

- [Gettext utils](https://www.gnu.org/software/gettext/): For maintaining content translations for the new .po/Gettext format
- [mod_translation_updater](https://github.com/luanti-org/modtools/blob/main/mod_translation_updater.py): Python script to create and update legacy .tr mod translation files

## ContentDB

- [cdbrelease](https://gitlab.com/sztest/cdbrelease): Release automation tool for ContentDB packages

## Misc.

- [Definition Ripper](https://gitlab.com/sztest/defripper): Extract node and item definitions, generating decorative and inert facades from them
