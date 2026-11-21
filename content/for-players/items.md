---
title: Items
aliases:
  - /Item_stack
  - /Items
  - /Unknown_Item
  - /engine/items
---

**Items** are things that can be stored in an [inventory](/for-players/inventory). This includes tools, weapons, [nodes](/for-players/nodes) and pure crafting elements that may or may not have additional behaviors ("craftitems").

## Item stack

An **item stack** is a collection of multiple items of the same type. Any item type has a stack limit, it is not possible to stack more items of such an item than the stack limit.

### Appearance

Item stacks can be stored in inventories and can also appear in the world itself.

#### In an inventory

In the inventory, item stacks are represented by an icon and optionally a number. The number is the size of the item stack. If there is no number, the size is 1. Refer to [inventory controls](/for-players/inventory#controls) for instructions how to use item stacks in inventories.

#### In the world

[](/images/items/Item_stacks_in_the_world.png)

Some dropped item stacks

Item stacks appear in the world as [objects](/for-engine-devs/objects). They are represented by some sort of symbol. For blocks, the icon is a rotating mini-version of it. Other things represented by the inventory icon. In the world, any item stack appears just as a single object, even with a stack size of 99. But item stacks with a larger stack size will appear larger than item stacks with a low stack size. An item stack can be collected by [punching](/for-players/punching) it. Item stacks are subject to gravity. If multiple item stacks of the same item occupy the same block position, they will merge into one object, as long the stack limit is not exceeded. Dropped items also have a life time. If they stay untouched in the world for too long, they disappear. The life time can be changed via [minetest.conf](/for-players/minetest-conf) (`item_entity_ttl`).
An item stack that is stuck in a solid block will be pushed outside of it until it reaches a non-solid block.

Dropped items can appear for many reasons:

- a player drops them
- a player mines a block, but there's no more space in the inventory
- a [mob](/for-players/mobs) or a player dies
- other events are possible

### Modding item stacks

Mods can change the stack limit, or the behavior of the dropped items.

For example:

- Item magnet: When a player stands close to a dropped item, it gets attracted like a magnet and will be automatically collected. Punching it is not required
- Water flow: Dropped items move with the flow of water
- Destruction: Dropped items get destroyed if they touch lava or fire

## Unknown Item

![](/images/items/Unknown_Item.png)

Inventory image of an unknown item.

An **unknown item** is a pseudo-item in Luanti to represent an item of which the item definition is unknown. These items should never appear in the game, and it's always an error when you encounter it.

### Behavior

Most types of world interactions are not possible. Dropping an unknown item is not possible. You can still store and move this item in inventories.

If you try to place, use or drop an unknown item, an error message will be shown to the server. This message is not visible if you are connected to a server as a normal player. The error message reveals the item's [itemstring](/for-players/itemstrings).

Internally, an unknown item still knows the “real” item it represents and the associated data. To fix problems with unknown items, check the troubleshooting section below. You can destroy unknown items with the `pulverize` [server command](/for-players/server-commands).

### Troubleshooting

A common reason for this item to appear is when you have previously activated a [mod](/for-players/mods) which added some new item, stored some of these items in an inventory, then later deactivated said mod. Now all items from this mod will appear as an unknown items. In this case, you can solve this simply by enabling the missing mod again. If you forgot to which mod this item belonged, right-click with the item in hand to see its itemstring. The part before the column is the mod name.

Another source of this item is a bug in mods or games or just general developer clumsiness. Developers of a game may have made a mistake or removed items intentionally without any replacement. Complain to the game authors if this happens, as this is generally considered poor development practice. If unknown items occur without you using any mods, this is almost certainly a bug.

### Technical background

Technically, an unknown item still kinda acts like a normal item, and most item query and manipulation functions will still work. If you query the itemstring of an unknown item with e.g. `itemstack:get_name()`, you get the itemstring which the unknown item represents. To find out in Lua whether an item stack is unknown, call `itemstack:is_known()`.

## See also

- [Nodes](/for-players/nodes)
- [Objects](/for-engine-devs/objects)
