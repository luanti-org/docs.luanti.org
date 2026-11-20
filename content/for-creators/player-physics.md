---
title: Player physics
aliases:
  - /Player_physics
  - /player-physics
---

**Player physics** refers to special physical attributes of the player that can be changed in Lua with `player:set_physics_override`. With this function, you can set e.g. jump strength, walking speed and gravity for each player.

## Basic usage

See `lua_api.md`. Seriously, read it. :P

## When things break

However, this method only can set the raw physical values, which will overwrite any previous value the player had.

As long you have only exactly one mod in your entire game that sets player physics using this method, this is not a problem. But as soon more than 1 mod messes with the same player physics attribute (e.g. speed), all hell breaks loose and hilarity ensues, because each mod now competes for the “correct” physics attribute. In the worst case, speed, jump strength or gravity will go all over the place.

This type of error occurs more often than you think. Modders that are new to Luanti had to constantly learn this the hard way, because this problem is not obvious to see.

### Example

Let’s say there is a simple platformer game which has nodes that can slow down the player or increase jump height while the player stands on them. These mechanics are implemented via `player:set_physics_override`.

Let’s say now you come along and want to add potions that make the player run faster or increase jump height even more. This mod also uses `player:set_physics_override` to set the physics values directly.

Congratulations! You have now successfully created a race condition! When the player drinks a potion and walks on a “high jump” node, both the mod and the game are now competing to set the correct jump/speed values, so the player physics will be completely broken because the values are overwritten chaotically.

### Solutions

#### Improve the Lua API by allowing player physics to be combined

The idea is to add functionality to the Lua API by allowing to set modifiers (like multiplication or addition), so mods can change physics without worrying at all about other mods.

This solution has strong community support, but sadly, it did not succeed yet.

[https://github.com/luanti-org/luanti/pull/7269](https://github.com/luanti-org/luanti/pull/7269)

So we’re currently stuck with mod-based solutions, sadly.

#### Player Physics API

Player Physics API is a mod by Wuzzy that basically implements what has been suggested for Luanti. It resolves this conflict by providing a “common ground” for mods to work together in this regard. You can find this mod here: [https://forum.luanti.org/viewtopic.php?f=11&t=22172](https://forum.luanti.org/viewtopic.php?f=11&t=22172)

It works by letting mods add and remove “modifiers” to a certain physical attribute, and the final result is created by multiplying all modifiers together.

The existence of this mod also expects that all mods (for a given game, that is) use Player Physics API instead of `player:set_physics_override`, otherwise, it won’t work.

This approach has been tried and tested in MineClone 2 and Repixture.

#### Create custom helper functions

If, for any reason, Player Physics API is not suitable for your game, you might want to create your own helper functions.

What you want to achieve is to have some “middleware” mod like Player Physics API that allows mods to somehow modify player physics without overwriting the final result. You then combine all values to calculate a final result and use it in `player:set_physics_override`. Which formula you use is up to you. Or maybe you can think of an entirely different solution.

It is not known if any alternative approaches (besides Player Physics API) have been done.

#### Only allow the game to set physics

In the game’s developer documentation, explicitly say that only the game is allowed to set physics, and that external mods aren’t. Resolve and in-game conflicts by hand. This is obviously the least flexible solution, but it's also the simplest. But for larger games with more complex physics, you will quickly run into problems.
