---
title: Mods
aliases:
  - /Mods
  - /mods
---

**Mods** (short for **modifications** or **modules**) are user-created modifications to a game in such a way that adds to or alters the gameplay. Some larger mods may add a lot of content to the game, while other smaller mods may add more settings/customization options or tweak the gameplay in small ways. [Server](/for-players/servers) mods or plugins mainly give server admins more options and ease of use, and all mods for singleplayer can also be used in multiplayer.

While Luanti mods are generally safe to install as they run by default in a sandboxed environment, one should exercise caution with mods as they may modify your world data in an undesired way. A good way to protect your game from such problems is to regularly [back up](/for-server-hosts/backup-solutions) your world folder(s) when installing new mods.

To browse the selection of mods available for Luanti games, see [ContentDB](https://content.luanti.org/packages/?type=mod).

## Modpacks

A modpack (short: “MP”) is a collection of mods to group them together. Basically, a modpack is just special directory containing the actual mod directories. It's purely a logical grouping and is done mostly for convenience and to group closely-related mods together. The main difference is that they will be displayed as a openable blue text item in the Luanti mod selector containing the component mods to allow granular control. Apart from that, there is nothing special about modpacks or mods inside a modpack. Individual mods in a modpack can still be enabled and disabled as if they were standalone mods.

## Client-Side Mods

A Client-Side Mod (short: “CSM”) is a mod used to customize your Luanti client. Client-Side Mods can be used when connected to a Luanti server because they are loaded locally. Client-Side mods require at least Minetest 0.4.15-dev from sources (note: Luanti was called “Minetest” back then), compiled after April 1st, 2017. Please note that the API is currently not stable and can change.

## Finding mods

Generally, all mods you would want are available on [ContentDB](https://content.luanti.org/). They can be installed from the main menu by going to the _Content_ tab and clicking on the _Browse online content_ button.

For old or experimental mods, check the [Mods](https://forum.luanti.org/viewforum.php?f=46) subforum.

## Installation

Starting with version 5.0.0, mods can be installed using the _Content_ tab in Luanti.

To manually install content for Luanti, see [How to install content](https://content.luanti.org/help/installing/).

## Listing actual running mods

The in-game [server command](/for-players/server-commands) `/mods` will list the active mods of the actual running game.

## Further reading

- [Creating Mods](/for-creators/creating-mods)
