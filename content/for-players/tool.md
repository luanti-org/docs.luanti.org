---
title: Tool
aliases:
  - /Tool
  - /tool
---

A **tool** is an [item](/for-players/items) which has some practical use to interact with the environment. Although some tools are unique, all tools have certain properties in common.

## Usage

How to use a tool often depends on the tool itself. Most tools are used with the attack key (Left mouse button) or the build key (right mouse button).

## Mining tools

Many tools are mining tools; refer to Mining for more information.

## Weapons

Tools which are able to damage opponents are called “weapons”. Since weapons share all properties of tools, weapons are considered tools, too. You attack with the attack key (obviously). Each weapon deals different weapons and it’s also possible that damage differs based on what you attack (since the target might be resistant to damage).

Weapons have a “full punch interval”, this is the time it takes between attacks to deal the full damage again. This is indicated by the weapon “swing” animation. If you attack after the full punch interval has passed, you deal the full damage. But it is possible to attack before that, but the damage dealt will be reduced (possibly down to 0). So you decide whether you attack quickly, but with low damage, or take your time between swings, and deal full damage.

## Wear

A tool may wear off when used often. Whether and how fast a tool wears off depends on the tool type. Each time you use such a tool it becomes a little bit worn off. This wiki often shows how often a tool can be used with the “uses” count. I.e. “20 uses means” the tool can be used 20 times before breaking. This number might not always be exact, it’s often more complex, so check out additional notes in the respective wiki pages.

You can tell how much a tool is worn off by a small colored bar below the inventory icon. A tool which doesn’t wear off or which is not worn off yet does not have such a bar under its inventory icon. When this bar becomes empty, the tool is destroyed. You might consider [repairing](#Repairing) a tool before it becomes destroyed.

Weapons get used up by attacks, mining tools get used up by mining, other tools get used up for other reasons. Some tools might not get worn out at all, however. The tool wear caused by damage is proportional to the time from the last swing, i.e. a quick, low damage attack wears out the weapon less, while full blows also add the maximum wear. The wear caused by mining increases with the “difficulty” of the block being mined, the higher the difficulty, the higher the wear. Blocks that could have been dug by the Hand (default tool) do not add to the tool wear.

In Creative Mode, tools generally do not wear.

### Repairing

A game can make a repairing recipe available. If this is the case, tools which wear off can be repaired by placing two tools of the some kind into the crafting grid. The recipe is shapeless. Other games and mods may change this percentage or even disallow repairing completely, or add a custom repairing method.

## Default tool

When you wield nothing at all (empty slot in highlighted hotbar slot) then you “wield” the default tool. All games have its own default tool and it may or may not be different. The default tool is not a tool because it’s not an item but it is called “default tool” because it shares many properties with tools.

The default tool never wears off and doesn’t behave like an item; e.g. it can’t be thrown away.

The default tool may or may not be a mining tool and it may or may not be a weapon. It is always possible with the default tool to collect stuff.

If you carry an item which is not a tool (for example: a stone), it has the same properties as you’d have with the default tool. If the default tool is a digging tool and you try to mine a block with a tool for which it wasn’t build for, the mining time equals the mining time of the default tool.

## Tools in Minetest Game

The available tools are:

- Hand – default tool; mines the weakest blocks, serves as a weak weapon
- Pickaxe – mines cracky blocks like Stone
- Shovel – mines crumbly blocks like Dirt, Sand, etc.
- Axe – mines choppy blocks like Trees
- Sword – hurts enemies
- Bucket – collects liquids
- Screwdriver – rotates blocks

In Minetest Game, repairing is enabled. You will receive a tool which is 2% less worn off.

## Tools in other games

Mining tools and melee weapons of all kinds are quite popular. But modders can still come up with tools which serve an entire different purpose.

In practice, the default tool is often a hand.

Modders may come up with their own ways to repair tools as well.

Theoretically, modders may even abuse the “wear-off bar” for purposes other than showing the tool’s wear.
