---
title: Light
aliases:
  - /Light
  - /light
---

In Luanti, **light** is entirely block-based, just like the rest of the world.

## Light level

As the world is entirely block-based, so is the light in the world. Each block has its own brightness.

The brightness of a block is expressed in a “light level” which ranges from 0 (total darkness) to 15 (as bright as the sun).

## Types of light

There are two types of light: Sunlight and artificial light.

### Artificial light

Artificial light is emitted by luminous blocks. Artificial light has a light level from 1-14.

### Sunlight

Sunlight is the brightest light and always goes perfectly straight down from the sky at each time of the day. blocks. At night, the sunlight will become moonlight instead, which still provides a small amount of light. The light level of sunlight is 15.

## Light transparency

Blocks have 3 levels of transparency to light/brightness:

- Transparent: Sunlight goes through limitless, artificial light goes through with losses
- Semi-transparent: Sunlight and artificial light go through with losses
- Opaque: No light passes through

Artificial light will lose one level of brightness for each transparent or semi-transparent block it passes through, until only darkness remains. Sunlight will preserve its brightness as long it only passes fully transparent blocks. When it passes through a semi-transparent block, it turns into artificial light.

Note that “transparency” here does not always mean you can see through a block. It only means that the block is able to carry brightness from its adjacent blocks.

## Light in [Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/)

### Examples of transparent blocks

- [Air](/for-players/nodes#air)
- Glass
- Obsidian Glass
- A few small plants

### Examples of semi-transparent blocks

- Water, River Water and Lava
- Leaves and the like
- Fences and the like
- Ice
- Mese Block
- Slabs and Stairs
- Doors, Trapdoors and the like
- A few other small plants

### Examples of opaque blocks

- Stone
- Wooden Planks
