---
title: Player-World interaction
aliases:
  - /Player-World_Interaction
  - /engine/player-world-interaction
---

## Digging a node

Preconditions are:

- a node somewhere in the map
- this node is pointable, has a selection box and can be dug by the hand or the tool the player is wielding.
- the player has `interact` [privilege](/for-players/privileges)
- the active tool has a tool range that the node can be [pointed](/for-players/pointing)
- the node is not stuck somewhere the player can't point it

1. First look at the node, it becomes `pointed_thing`. Note that on android the node is pointed without looking (referring to the cross position) at it.
1. Then LMB is pressed down while holding an item which does not have `on_use`. If LMB wasn't released before, a click happens, which causes a punch event.
   1. The client sends a punch event (`client->interact(4, pointed)`, see `game.cpp:3657` (28.02.2017)).
   1. On server side the punch event is passed to mods, see `on_punch` and the deprecated `register_on_punchnode`.
   1. Then the client sends the digging start event to the server (`interact(0, pointed)`, `game.cpp:3948`).
   1. The server uses this information for anticheat measurements.
1. Now continue looking at the node, i.e. keep pointing it, and hold down LMB.
   1. Periodically client-side: Crack animation is updated, digging particles are spawned (if not disabled in the settings) and the node dig sound is played.
   1. The dig sound is either the "dig" field in the "sounds" table in the nodedef
   1. If it's not present, the client uses the sound `default_dig_${group_name}` (file name extension omitted here), where `$group_name` is one of the groups the node has, see (`game.cpp:4001`), and `gain` is set to `0.5`.
1. Some time later, when the digging time elapsed, the client sends a digging completion event to the server (interact(2, pointed), game.cpp:4018), the node disappears client-side, more particles are spawned (if enabled) and the dug sound is played.
1. Now the server receives the digging completion event. Anticheat probably, e.g due to lag, thinks the player dug the node too fast.
   1. `dug_too_fast` cheat is passed to mods, see `register_on_cheat`, and digging aborts.
   1. If the `nodedef` has a `can_dig` function, it's executed and probably stops the digging.
   1. The `on_dig` function in the nodedef is called now.
   1. The default `on_dig` function probably removes the node:
      1. The `on_destruct` is executed, air is set and `after_destruct` is executed.
      1. It then calls the `after_dig_node` function in the `nodedef` if present.
