---
title: Mod Dependency Management
aliases:
  - /mod-dependency-management
  - /content-dev/mod-dependency-management
---

# Mod Dependency Management

A mod has the ability to depend on other mods, either required dependencies or optional dependencies. These are specified as comma separated lists in the mod's `mod.conf` file, `depends` and `optional_depends` respectively.

A mod's dependencies get loaded before the mod, such that API functions get defined and are available for use. If no mods have dependencies, mod loading order goes in reverse alphabetical order, but this is undocumented and you should not rely on this - you can use the `random_mod_load_order` setting to shuffle the order to make sure your game or mod still functions.

## Required dependencies

Required dependencies need to be resolved for a mod to be allowed. You can always rely on these depending mods to exist. In recent versions of Luanti, the game will refuse to load if a player attempts to load a mod with unresolved required dependencies. If your mod gets installed from the main menu content browser, it will attempt to also download required dependencies.

## Optional dependencies

Optional dependencies can be used to provide additional functionality if a mod exists. If you want to use API functions of the mod, you will need to check if the mod exists using `core.get_modpath` (it will return nil if the mod isn't loaded).

```lua
if core.get_modpath('optionally_dependent_mod') then
    -- Code for when optionally_dependent_mod exists
end
```

## Game Support

In order to support several games, you would want to optionally depend on each game's specific mods. For example:

```lua
if core.get_modpath("default") then
    -- Code path for Minetest Game
elseif core.get_modpath("mcl_core") then
    -- Code path for Voxelibre or forks
end
```
