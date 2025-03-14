---
title: Minetest.conf
aliases:
  - /Minetest.conf
  - /minetest-conf
---

# Minetest.conf

**Note:** Since version 0.4.14, Luanti has an “advanced settings” feature to change virtually all settings in Luanti, including a brief description of what each setting does. You will now rarely need to edit minetest.conf manually.

**minetest.conf** is the configuration file used for numerous purposes. This file is read every time the game starts and is always created/modified when the menu quits.

A file called [minetest.conf.example](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example) is provided as an example. See that file for a more complete list of options.

## Location

If you are running a portable build of Luanti (aka RUN_IN_PLACE=1, user data is stored alongside the executable), it loads from:

- `../minetest.conf`
- `../../minetest.conf` (it can load settings from this location too, but will not write to it)

If you are running a system-wide build of Luanti (user data is stored separate from the executable), it loads from:

- `~/.minetest/minetest.conf`

A custom path to a configuration file can be specified in command line using the option `--config /path/to/minetest.conf`.

## Controls

In `minetest.conf`, you can configure most (but not all) keys, but a few more keys can be configured which are not configurable in the in-game settings menu; most notably, camera mode and minimap.

A few things to note:

- Controls are written in the format `keymap_<action name> = <key name>`, e.g. `keymap_forward = KEY_KEY_W`
- Starting from 5.12.0, the key name is defined as `SYSTEM_SCANCODE_num`, where `num` is the [_numeric_ scancode for the key](https://wiki.libsdl.org/SDL2/SDL_Scancode). Note that `sdl2-compat` uses [scancodes from SDL3](https://wiki.libsdl.org/SDL3/SDL_Scancode) which is different for certain keys. The old syntax based on Irrlicht keycodes and literal characters is deprecated (except for keycodes representing mouse buttons, e.g. `KEY_LBUTTON`) and should no longer be used.
- For versions earlier than 5.12.0, the list of possible controls (value right of the equals sign) can be seen in [\[1\]](https://github.com/luanti-org/luanti/blob/master/irr/include/Keycodes.h). If your key isn't listed there, try writing the literal character
- To disable a control completely, leave the part right of the equals sign empty, e.g. `keymap_toggle_debug =`

See [controls](/for-players/controls) for a list of controls and their setting name (if available).
