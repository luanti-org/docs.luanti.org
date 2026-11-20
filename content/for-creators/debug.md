---
title: Debug
aliases:
  - /Debug
  - /debug
  - /content-dev/debug
---

The **debug screen** can be accessed by pressing the F5 key (by default) and shows various technical information about Luanti which are mostly interesting for developers, but some information are also useful for players, especially the [coordinates](/for-players/coordinates).

It contains various information useful for development and testing. Press F5 to access different debug screen modes:

- Debug screen disabled
- Debug info
- Debug info + profiler graph
- Debug info + wireframe (only with “debug” [privilege](/for-players/privileges))

## Debug info

### Basic debug info

The basic debug information is located at the top left of the screen. It looks like this:

[![Debug.png](/images/Debug.png)](/images/Debug.png)

#### First row

- **Luanti X.Y.Z**: The [version number](/for-engine-devs/version-number) of [Luanti](/about/luanti)
- **FPS**: Number of [frames per second](https://en.wikipedia.org/wiki/Frames_per_second), denotes how fast everything is [rendered](https://en.wikipedia.org/wiki/Rendering_%28computer_graphics%29). Higher = better. It is limited at 60 by default. An FPS lower than 30 is pretty bad (try to lower graphics settings or close some background applications)
- **drawtime**: An average time (in milliseconds) it's taking to render each frame, not including processing other than rendering. Lower is better
- **dtime jitter**: Jitter in the time difference between rendering frames, including all processing. Luanti remembers the previous drawtime values over a few seconds in the past. This value shows how much higher than the average the _peak_ value (over the last few seconds) was. A value of 50% or lower is considered okay
- **view range**: Your current viewing range in nodes. “All” means unlimited. By default, you can adjust this with +, \- and R.
- **RTT**: [Round Trip Time](https://en.wikipedia.org/wiki/Round_trip_time). This is especially important when connected to a server. Lower is better

#### Second row

- **pos**: Your [coordinates](/for-players/coordinates): X, Y and Z
- **yaw**: Your current horizontal looking direction (also known as “yaw”). For convenience, also the cardinal direction (e.g. “North”) as well as the approximate axis direction are shown (e.g. “+Z”)
  - 0° translates to “North”, 270° to “East”, 180° to “South” and 90° to “West”. Note that the concept of cardinal directions does not really make sense in Luanti, as the world is a cube, not a sphere, and there are no poles. In this context, the names “North”, “South”, “West” and “East” are just synonyms for the 4 directions
- **pitch**: Your current vertical looking direction (i.e. “pitch”). 0° means you look horizontally, positive numbers means looking upwards and negative numbers means looking downwards
- **seed**: The [random seed](https://en.wikipedia.org/wiki/Random_seed) used by the [map generator](/for-creators/mapgen) to generate the current world. Equal seeds (along with equal mapgen settings) will lead to equal worlds
- **pointed**: The [itemstring](/for-players/itemstrings) / “technical name” of the current [pointed](/for-players/pointing) [node](/for-players/nodes) (if any).
- **param2**: Value of `param2` of the current pointed node (if any). This contains some additional info for a node, such as rotation, color, etc, which is important for programmers. The meaning of `param2` is explained in the Lua API documentation.

Note: The entire second row is hidden when the debug view is restricted (see below).

### When pointing an entity

When you point an entity or object (such as a dropped item or player), the following information is shown to the left:

- Entity type: See [ActiveObject](/for-engine-devs/objects/#activeobjects) on the Minetest Developer Wiki
- **hp**: [Health](/for-players/player/#health) in hit points
- **armor**: Armor groups, determine how the entity receives damage (see below)

Note: This is hidden when the debug information is restricted (see below).

#### Armor groups

The armor groups are used internally by Luanti to tell how “vulnerable” an entity (including players) is to different forms of attack. Each armor group has a name and a number. The number is a percentage, 100 means you get 100% of the damage, 200 means you get double the damage, 50 means you get 50% of the damage, etc. Any armor group that is \*not\* in this list means that the entity does not take damage from attacks of this group.

By default, the armor group “fleshy=100” is used for entities. Most armor mods use these armor groups to reduce damage. Minetest Game only uses this armor group for both weapons and entities, and so do many other games. The name “fleshy” does not mean the entity is actually made of flesh, this is just the default name.

But some games might use more complex systems. Think, for example, of elemental creatures that are vulnerable to ice-based attacks but immune to physical attacks.

The armor group “immortal=1” is a special case and means the entity does not take any damage by conventional means (however, mods might still manage to handle damage in a completely different manner).

### Profiler graphs

The profiler graphs show the performance of Luanti in a more detailed fashion. This information is most useful for engine developers.

See [Profiler Graph](/for-engine-devs/profiler-graph) for more information.

### Wireframe

If this mode is active, the world will be drawn as a [wireframe](https://en.wikipedia.org/wiki/Wire-frame_model), which reveals the technical structure of the 3D models in the game.

Because for players, this is basically an X-ray vision and very overpowered, access to the wireframe mode is only possible if you have the “debug” privilege.

## Restricted debug view

A few games and mods might restrict the amount of debug information by hiding gameplay-relevant information like the coordinates. If you have the “debug” privilege, this does not affect you, and you always get to see the full debug information. By default, the debug screen is _not_ restricted in this way; games or mods have to actively decide to restrict it. For example, Minetest Game does _not_ restrict the debug view.

This is a list of things that will be hidden/restricted in a restricted debug view:

- Position, yaw, pitch, seed, pointed node and param2 of pointed node
- Entity information (type, hp, armor groups)
- The ability to display mapblock bounds

The reason why a game might choose to restrict the debug view is to make it more challenging, like if the game wants players to “earn” the ability to see coordinates or other info by crafting a special item first. Otherwise, players could just skip that with only one keypress, which would fly in the face of game design.

In singleplayer mode, remember you can always say “/grantme debug” in chat to get the full debug info back and ignore all game restrictions (this is often considered a cheat).
