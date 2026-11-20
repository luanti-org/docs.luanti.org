---
title: Player
aliases:
  - /Player
  - /player
---

The **player** or **“player character”** is the character that a user controls.

## Health

Players start with 10 hearts, which is equal to 20 hit points (HP). The smallest unit of health is 0.5 hearts or 1 HP. Players die when they have lost all hearts.

Players can restore health by consuming food. Players can lose health by various means, including, but not limited to: Falling too hard, touching a harmful block such as lava, drowning in a [liquid](/for-players/liquid) or getting attacked by other players (if PvP is enabled).

The built-in health system can be completely disabled by the game or mods, or by the player through the per-world `enable_damage` setting if made accessible by the game.

## Breath

Players have up to 10 breath points, represented by bubbles. Players start at 10 breath points and normally the breath quickly increases up to 10 in steps of 1 breath point. If players have full breath, the breath meter in the HUD is usually hidden. While the player is inside a [liquid](/for-players/liquid) with drowning damage, breath will be reduced by 1 every 2 seconds. If the breath reaches 0, the player will take damage every 2 seconds as long the player is inside the liquid. The damage taken depends on the liquid type.

The built-in breath system can be completely disabled by the game or mods as part of the health system, or by the player through the per-world `enable_damage` setting if made accessible by the game.

## Death

When players lose their health, they die. After death, the player can immediately respawn, with health and breath restored. The new spawn position is usually somewhere near the initial spawning position (see [Spawn Algorithm](/for-creators/player-spawn-algorithm/)). Other consequences of death (usually negative) can be implemented by mods.

Default engine behavior is that the player keeps the items that are in their inventory after death, but this behavior can be changed by games and mods.

## Name

Player names may consist only of the characters a-z, A-Z, 0-9, the dash (“-”) and the underscore (“\_”). Other characters are not allowed.

## Appearance

The default appearance of the player character in the engine is as a flat 2D sprite with a front and back texture. The "Change Camera" keybind can be used by the player to switch into third-person mode to see their character in the world from the back and from the front. It is possible for games to replace the sprite textures, or replace the character completely with a custom animated 3D model.

## Trivia

- The default player skin in `player_api` commonly used in Minetest Game and other games, is named "Sam". It is a recursive acronym for "Sam ain’t Minecraft".
