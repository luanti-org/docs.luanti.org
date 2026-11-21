---
title: Groups
aliases:
  - /Groups
  - /groups
---

**Groups** is a concept for [nodes](/for-players/nodes), [items](/for-players/items) and [tools](/for-players/tool) in Luanti. Any of these may be a member of a group or multiple groups, or none at all. Groups are used mainly for crafting recipes, to determine damage, to determine digging time and lots of other stuff as well.

> [!NOTE]
> Below are some sample groups from [Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/). [Mods](/for-players/mods) often remove or heavily modify these groups, so they may not apply to other games.

## List of ordinary groups used by crafting recipes

- wood
- stone: Stone, Cobblestone, Stone Brick, Desert Stone, Desert Stone Brick
- sand: Sand, Desert Sand
- flora: Flowers (White Dandelion, Yellow Dandelion, Blue Geranium, Rose, Tulip, Viola), and other small plants
- leaves

## List of known groups that determine damage and digging time

{{% comment %}} cspell:disable {{% /comment %}}

- oddly_breakable_by_hand: can be broken by [hand](/for-creators/hand).
- crumbly: stuff like dirt and sand. Shovels are great to break nodes in this group.
- cracky: tough stuff like stone. Pickaxes are great to break nodes in this group.
- choppy: something that can be cut using force; eg. trees, wooden planks. Axes are great chopping these down.
- fleshy: living things like animals and players. Swords deal great damage to them.
- snappy: something that can be cut using fine tools; eg. leaves, small plants, wire, sheets of metal. Swords can be used to dig these, but they wear out quickly.
- explody: especially prone to explosions.
  {{% comment %}} cspell:enable {{% /comment %}}

## List of special groups (excerpt)

- dig_immediate: block can be immediately or almost immediately be dug.
- soil: saplings will grow on blocks in this group.
