---
title: Itemstrings
aliases:
  - /Itemstrings
  - /itemstrings
---

**Itemstrings** are the internal names for items, blocks, nodes (everything which can be stored in an [inventory](/for-players/inventory)).

## Syntax

An usual itemstring consists of a the name of the [mod](/for-players/mods) where the item originates from followed by a “:” followed by an item name. All itemstrings are case sensitive.

```
<mod_name>:<item_name>

```

Examples:

- `default:torch` — a torch (from mod “default”)
- `default:dirt` — dirt (from mod “default”)
- `farming:bread` — bread (from mod “farming”)
- `wool:orange` — orange wool (from mod “wool”)
- `screwdriver:screwdriver` — screwdriver (from mod “screwdriver”)

There are itemstrings which don’t follow the syntax rules, see [#Special itemstrings](#special-itemstrings).

## Usages

Itemstrings can be used as arguments used for the `/give` and `/giveme` [commands](/for-players/server-commands).

Example: `/giveme default:torch`—give yourself a torch

## Special itemstrings

There are itemstrings which do not follow the usual syntax rules. These itemstrings are always available, regardless of the activated [mods](/for-players/mods):

- [`air`](/for-players/nodes/#air)
- [`ignore`](/for-players/nodes/#ignore)

## See also

- [Items](/for-players/items)
- [Nodes](/for-players/nodes)
