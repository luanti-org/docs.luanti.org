---
title: Game Theme Re-implementation
aliases:
  - /game-theme-reimplementation
---

The part of the main menu that controls the game theming aspects is implemented with `mm_game_theme`, in `game_theme.lua`. If you wish to rebrand the main menu, dealing with the complexity of this implementation can be rather unnecessary if you just want a single background and header that shows up on every tab.

What `mm_game_theme` does is essentially just abstract away calls to `core.set_background`, a function which controls background textures, but also header and footer textures. See the entry for it in `menu_lua_api.md`:

```
* `core.set_background(type,texture_path,[tile],[minsize])`
  * `type`: "background", "overlay", "header" or "footer"
  * `tile`: tile the image instead of scaling (background only)
  * `minsize`: minimum tile size, images are scaled to at least this size prior
   doing tiling (background only)
```

This is an example of a minimal reimplementation (from Voxelmanip Classic) that shows a static, untiled background and a header from the base textures directory in the main menu:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
core.set_background('header', defaulttexturedir .. "menu_header.png")
core.set_background('background', defaulttexturedir .. "menu_bg.png")
core.set_clouds(false)
```

{{% comment %}} cspell:enable {{% /comment %}}

This would be put in the `init_globals()` function in place of calls to `mm_game_theme`.
