---
title: Right to a Name
aliases:
  - /right-to-a-name
---

Right to a Name (abbreviated RTAN, alternatively RTaN) is a policy enforced by ContentDB where every modname (the technical name of a mod) needs to be unique. For the official policy see [Package Inclusion Policy and Guidance § 3.1. Right to a Name](https://content.luanti.org/policy_and_guidance/#31-right-to-a-name).

## Motivation

The reason for its enforcement is due to the Luanti dependency system requiring unique technical names and can't distinguish several mods with the same technical name. They will be treated as the same mod and any mods depending on it would break if the wrong incompatible version was enabled.

## Checking

ContentDB has a page where you can check whether a modname exists, both on ContentDB and on the Luanti Forums.

```txt
https://content.luanti.org/modnames/<modname>/
```

If you have uploaded your package, an alert will pop up on the package page if it contains technical names that already exist on ContentDB.

!["Please make sure that this package has the right to the names it uses."](/images/right_to_a_name/cdb_warning.webp)

Just because this message shows up doesn't necessarily mean you are violating the policy, as the name conflicts could fall under the exceptions mentioned below. The message is also used for ContentDB Editors to quickly check the validity of technical names used in a package during review.

## Exceptions

The rule does not apply when a mod is included verbatim in a game. If the mod is lightly modified, then it should be fine too. Generally the mod should be roughly equivalent to its upstream counterpart API-wise, such that other mods depending on it still function.

This also means that re-implementations of mods are exempt, as they could serve as a drop-in replacement for the mod's API.

## Game namespacing

It is standard practice when making games to prefix your game's modnames such that they don't have any risk of conflicting with other modnames. For example, NodeCore uses the `nc_` namespace. A list of namespaces used by games on ContentDB can be seen at (TODO).

Game namespacing doesn't apply for Minetest Game's mods, assuming that they are compatible with mods that depend on Minetest Game's mods and aren't heavily modified.
