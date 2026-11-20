---
title: Pointing
aliases:
  - /Pointing
  - /pointing
---

**Pointing** means to look at something with the crosshair (which is in the center of your screen) on it while being close enough to it. When something is pointed, it is highlighted, either by a wireframe or by some sort of “halo”. Pointing things (aka pointing **at** things) is necessary for tasks like mining, building, using items, etc.

The default distance for pointing is less than 4 blocks away. In Minetest Game, this distance increases to 10 when using creative mode. It is possible for items and held nodes to have their own pointing range.

Most blocks can be pointed at, but not all. Liquids are usually not be pointed at, but there are special items which are capable of pointing to liquids. Examples are buckets and boats. A few blocks like Air can never be pointed. Also, items on the ground can be pointed (in order to collect them).

It is only possible to point at one thing at a time.

By default, a thin black rectangular wireframe will be shown around the block or entity which you currently point. The colour and thickness of the selection box can be changed with `selectionbox_color` and `selectionbox_width`. It is also possible to change it from an outline to a highlight which tints the pointed node with a configurable colour.
