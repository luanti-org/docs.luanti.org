---
title: List of hardcoded features
aliases:
  - /List_of_hardcoded_features
  - /list-of-hardcoded-features
---

This is a **list of hardcoded features** in Luanti. A hardcoded feature is a feature with a certain functionality that behaves always in a certain fixed way that cannot be customized. Luanti in general wants to allow its mods and games to customize pretty much everything, so in the long run, any hardcoded feature should be made customizable. Note not all hardcoded features are created equal, some are less annoying than others, while for some features it really doesn't matter that they are hardcoded. Also, this list should be more seen as a documentation, not as a feature request to un-hardcode everything in this list.

## Gameplay

### Health and damage

- The fall damage calculation formula (issue: [#10348](https://github.com/luanti-org/luanti/pull/10348))
- Nodes that deal direct damage do so always once per second (with `damage_per_second`); no other time interval is possible
- How breath works in general (issue: [#8042](https://github.com/luanti-org/luanti/pull/8042))
- You lose breath or take drowning damage every 2 seconds, and you regain breath every 0.5 seconds
- The amount of time the `damage_texture_modifier` is applied when an object takes damage is hardcoded

### Items

- The formula that determines how fast your tool will dig
  - You can change the parameters, but not the formula itself
- The formula that determines how much your tool will be worn
  - You can change the parameters, but not the formula itself
- All items can point (issue: [#6651](https://github.com/luanti-org/luanti/pull/6651))
  - `range=0` does not work as you can point when you're inside a node

### Players

- All players have **all camera modes**, always. They can not be disabled or forced (issue: [#12855](https://github.com/luanti-org/luanti/issues/12855))

## Graphics

- When you take damage, the screen flashes red
- When you take damage, the screen shakes
- Sunlight has a subtle yellowish tint
- Darkness has a subtle blueish tint
- The light curves
  - This means how bright each individual light level will appear. Probably tricky to customize since the client also has a huge say in this
- The color of light
  - This does _not_ refer to colored lamps, but to the color of light globally. Imagine a game in which the sun is red, tinting the world in red, or a game in which _all_ lamps emit green light for some reason
- The 3D version of an item inventory image (fixed rotation, fixed param2)
  - `inventory_image` only supports 2D images
- Node digging particles (amount, size, lifetime, etc.)
- Wielded item / hand allows only very limited graphical customization (fixed angle, can't set 3D model explicitly, etc.)

## HUD

- Chat cannot be moved (by mods)
- Status message (for messages like “Fly mode enabled”) cannot be moved or hidden (by mods)

### Formspecs

- No or incomplete styling/theming/customization for ([#8744](https://github.com/luanti-org/luanti/issues/8744)):
  - Scrollbars
  - Drop-down lists
  - Highlight color of selected text
  - Text shadow
  - (possibly others)

## Controls

- Mouse wheel up and mouse wheel down are hard-bound to hotbar next/previous slot selection, and cannot be rebound to a different action. (But you can bind keys for this action, in addition to the scroll wheel)

## Hardcoded things that are probably OK

- The Esc is the pause/exit key
- The contents of the debug screen (note that whether to show basic info or everything is customizable)
- WASD keys or joystick for movement
