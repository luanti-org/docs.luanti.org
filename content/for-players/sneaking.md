---
title: Sneaking
aliases:
  - /Sneaking
  - /sneaking
---

Sneaking is part of the basic [controls](/for-players/controls) in Luanti with many useful capabilities. Normally, sneaking slows you down and protects you from accidentally falling off ledges. In some games, sneaking allows you to use powerful “sneak ladders” which can be climbed super fast.

Sneaking is a surprisingly diverse move; this article will explain all the details.

## Controls

The default key for sneaking is Shift. You will sneak as long as you hold down the sneak key. You can not sneak while you are at any climbable block (like a ladder), inside a [liquid](/for-players/liquid) or have activated [Fly Mode](/for-players/controls#movement-modes).

## Capabilities

Sneaking has many capabilities, some of which are not available in all games:

- **Slowdown**
- **Fall-off protection**
- **Sneak Glitch**
- **Sneak Jump**

Each game or mod can vary in what sneaking capabilities are actually available. By default (i.e. if the game code did not make any changes), the Slowdown and Fall-off Protection capabilities are enabled, but the others are not. Also, games can change the sneaking capabilities on a per-player basis.

### Slowdown

If you sneak, you will automatically walk slower. This is the most basic capability and available in almost all games.

### Fall-off Protection

![](/images/sneaking/Fall-off_protection.png)

Sneaking can stop you from falling over a ledge.

The Fall-off Protection is another very common feature of sneaking. If you try to walk beyond the edge of a cube while holding down the sneak key, you won't fall. You can walk a bit beyond the block's edge while sneaking, up to a certain limit, of course. This is very useful to safely walk at places with steep drops, like cliffs.

For this to work, you normally need enough space to stand on. You can't sneak if you have only one block of empty space (not enough for the player to stand normally). However, this rule does not apply if the Sneak Glitch is available.

In a few games, sneaking does _not_ have the Fall-off Protection. In these games, you will simply fall off the block if you sneak to the ledge.

### Sneak Glitch

![](/images/sneaking/Sneaking_at_the_edge.png)

Without the Sneak Glitch, you would fall down here otherwise

The Sneak Glitch is probably the most unique capability.

Without the Sneak Glitch, sneaking won't stop you from falling off a ledge if there is only 1 block of space. You would just fall down. However, _with_ the Sneak Glitch in place, this rule no longer applies, meaning you won't fall off the ledge even if there's only one block of space. The screenshot demonstrates this.

The Sneak Glitch also enables a **very** powerful move which allows you to climb so-called sneak elevators very quickly.

Because the Sneak Glitch is so powerful, it is not available in all games.

#### Sneak elevators

![](/images/sneaking/Sneak_bug_2.png)

A simple sneak elevator in a wall

![](/images/sneaking/Sneak_bug.png)

Alternative sneak elevator in a corner formation

A sneak elevator (also called “sneak ladder”) is a vertical structure that can be climbed up very fast if you have the Sneak Glitch capability.

In its simplest form, a sneak elevator is just a vertical line of solid blocks which are spaced apart by one empty block (like [air](/for-players/nodes#air)) each, so that empty and solid blocks are alternating. Sneak elevators can be built very easily built into high flat walls and cliffs, because you can continue to dig out the holes for the sneak elevator while climbing up. Sneak elevators don't have to be that simple, there are some variants that also work (see screenshots).

To climb up a sneak elevator, stand in front of the sneak elevator, then hold down both the sneak key and the jump key. You will move up the entire structure at an insane speed. This only works if you have the Sneak Glitch capability.

### Sneak Jumping

With this capability, you will jump slightly higher when you were sneaking. Just hold down the Sneak key and jump.

The increased jump height of a sneak jump might help you climb some obstacles you couldn't climb with a simple jump.

For game/mod programmers: Internally, this capability is functional if `new_move` equals `false` (don't ask why).

## Sneaking in Minetest Game

In [Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/) 5.4.0, sneaking has the Slowdown and Fall-off Protection capabilities. But there's no Sneak Glitch or Sneak Jumping.

## History

Sneaking used to be very buggy, but it has improved over time.

### Old bug: Avoiding fall damage

Up to version 0.4.15, you don't suffer any fall damage if you fall on a normal block's edge or on a slab or snow or on some other non-cubic shapes while holding down the sneak key. This bug is known as [issue 329](https://github.com/luanti-org/luanti/issues/329) and is fixed since version 0.4.16.

### The Sneak Glitch was a bug

The Sneak Glitch was considered to be a bug before (hence the name), and it caused many other, and it was also impossible for games to get rid of it. I.e. you could not make a game in which sneak elevators would not work. Because of this, the Sneak Glitch used to be a thing in Minetest Game.

The Sneak Glitch was fixed (=removed) from the game in version 0.4.16 due to many technical problems, but in a later version (around 2017), it returned as a proper feature, just disabled by default. The Sneak Glitch always had a big fan base, because it was a powerful move, so unsurprisingly there was kind of an uproar when it got “fixed”.

Today, Sneak Glitch is still called "Sneak Glitch" but it actually is not a glitch anymore. It used to be a glitch (unintentional broken feature) but now is an officially recognized feature in Luanti. The name “Sneak Glitch” just stuck.
