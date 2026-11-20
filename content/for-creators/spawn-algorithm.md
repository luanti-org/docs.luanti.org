---
title: Player Spawn Algorithm
aliases:
  - /Spawn_Algorithm
  - /spawn-algorithm
---

The **player spawn algorithm** (or **spawn algorithm** for short) tries to find a suitable spawn or respawn position for players. It is run whenever a player spawns or respawns.

This page describes how Luanti's builtin spawn algorithm works, as of **version 5.7.0**.
Note that individual mods and games can choose to override the spawning behavior (Minetest Game notably does).
The setting `static_spawnpoint` can also override it.

## Overview

If the setting `static_spawnpoint` is set to a valid position, Luanti will spawn new players at this position.

If this setting is not set, and no mod introduces its own spawning behavior, Luanti will apply an algorithm to find a spawn point automatically.

It works like this:

Basically, Luanti checks random position in a square centered on (0,0). Then it tries to find a suitable Y height to spawn the player in. If it did, that position is the spawn position. Otherwise, it increases the square size and tries again (with a new random position in that square).

The first square has a size of 3×3. If the first attempt failed, the search radius increases by 1, now it picks a random position in a 5×5 square (still centered on (0,0)). This is repeated until a valid spawn position is found. If the search square becomes too big or out of mapgen limits, the spawn search aborts and (0,0,0) will be used as the spawn position as a desperate fallback.

The algorithm makes up to 4001 attempts before giving up.

Note: This is a slight simplification of the actual code. The actual code for this is the function `Server::findSpawnPos` in `server.cpp`.

## Spawn Level

The spawn level is the Y coordinate of a XZ position at which players will spawn.
The particular algorithm/formula for this depends on the mapgen. The general goal here is to spawn the player on top the terrain.

A rough overview of spawn levels:

| Mapgen                      | Spawn level                                                                                           | Failure condition                                              |
| --------------------------- | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| v6                          | Base terrain height                                                                                   | If below or at water level, or 16 nodes above water level      |
| fractal                     | Lowest 3 consecutive air nodes, starting from Y=0, or water level if the 'terrain' mapgen flag is set | If no suitable air nodes were found for 4096 consecutive nodes |
| singlenode                  | Always 0                                                                                              | None                                                           |
| v5, v7, valleys, carpathian | It's complicated...                                                                                   | It's complicated...                                            |
