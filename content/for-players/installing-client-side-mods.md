---
title: Installing Client-Side Mods
aliases:
  - /Installing_Client-Side_Mods
  - /installing-client-side-mods
---

## Security considerations

**Prior to installing any [Client-side mods](/for-players/mods/#client-side-mods), please make sure that you have received it from someone you trust. Malicious code can damage your computer, violate your privacy or cause your computer to take part in illegal activities.**

## Installing a Client-side mod

After downloading a client-side mod (e.g. from the [Client-side modding forum](https://forum.luanti.org/viewforum.php?f=53)) you usually have a Zip archive. In order to get the mod running, you have to unpack it into the clientmods folder.

You may have to change the folder name to the “technical” mod name (e.g. rename `colour_chat-master` to `colour_chat`). You can usually find the mod name in the title of the forum topic—It is the last name within the square brackets in the topic title. For example, if the title is `[clientmod] Lol Mod [1.0] [anotherlolmod]`, then the folder must be renamed to `anotherlolmod`.

If one of the below mentioned directories does not exist, create it.

## Installation directory

The common place to install them is `$path_user/clientmods/`. That is `luanti-install-directory/clientmods/` in the official Windows releases and on GNU/Linux with `RUN_IN_PLACE` enabled and **`~/.minetest/clientmods/`** in globally installed Luanti versions.

- Location of the clientmods folder within the folder structure of a run-in-place installation of Luanti, including some of the folders Luanti adds after some usage as client and server, as well as the positions (…) that custom-made content goes. Irrelevant folders are not expanded.

```
luanti/
├── bin/
├── builtin/
├── cache/
├── client/
├── clientmods/
│   └── … (installed extra clientmods)
├── doc/
├── fonts/
├── games/
│   ├── minetest_game/
│   ├── minimal/
│   └── … (installed extra games)
├── locale/
├── mods/
│   └── … (installed extra mods and modpacks)
├── textures/
│   ├── base/
│   │   └── pack/
│   └── … (installed extra texture packs)
└── worlds/
    └── … (saved worlds. Some with exclusive world mods)

```

After extracting the Client-side mod there, you first need to tell it to load. This is done by adding `load_mod_<clientmodname> = true` in the mods.conf file in the clientmods directory. Next, you need to make sure Client-side modding is enabled. To do this, click on “Advanced Settings” in the “Settings” tab of the Main Menu. In the search-bar, type `client modding` and press “Search”. If “Client modding” is disabled, double-click it or press “Edit” and use the drop-down to enable it.

Note that Client-side mods are enabled for all worlds and servers.

## Example structure

In this example the Client-side mods “`colour_chat`”, “`csm_who_plus`”, and “`debugger`” are installed:

```
    clientmods/
    ├── colour_chat/
    │   ├── init.lua
    │   └── README.md
    ├── csm_who_plus/
    │   ├── init.lua
    │   ├── LICENSE.md
    │   ├── README.md
    │   ├── screenshot.png
    │   └── screenshot2.png
    ├── debugger/
    │   ├── doc/
    │   │   ├── screenshots
    │   │   |   └── …
    │   │   └── form_editor.md
    │   ├── textures/
    │   │   └── debugger_form_editor.png
    │   ├── init.lua
    │   ├── LICENSE
    │   └── README.md
    └── mods.conf

```
