---
title: Worlds
aliases:
  - /Worlds
  - /worlds
---

A **world** contains an environment and/or building(s) you can play in. A world also includes all saved data associated with a world, like player data, mob positions, health, breath, and the like.

## Finding worlds

See the [Maps forum](https://forum.luanti.org/viewforum.php?f=12) on the Luanti forums to find downloads to worlds that others have made.

## Installation

**To install worlds:** You have to extract them first. Most of them are in `.zip`, some of them can be in `.7z`, `.rar` or `.tar.gz` format. To extract archive files other than `.zip` on Windows, you need [7-Zip](https://7-zip.org/).

For world creators, `.7z` is the recommended archive format as it is a free format (as compared to `.rar`) that typically allows for the best compression.

Put the extracted files in the `worlds` folder of your Luanti installation folder. To find the folder, select the "About" tab in the main menu and press "Open User Data Directory".

The files such as `env_meta.txt` **must** be in the root of the world’s folder (eg. `worlds/my_world/env_meta.txt`).

## World directory content

See [world_format.md](https://github.com/luanti-org/luanti/blob/master/doc/world_format.md) in the Luanti source tree.

## Schem file Creation / Import

A **schem file (`.mts`)** is used to import building(s) into a world with the [WorldEdit mod](https://content.luanti.org/packages/sfan5/worldedit/). This file can be found in “`worlds/<my_world>/schems`” folder.

- To **create a schem file** :

1. Type in your world name (with WorldEdit activated).
2. Grant yourself all [privileges](/for-players/privileges/): `/grantme all`
3. Press F5 to show the coordinates.
4. Select the area to export by commands with `//pos1` and `//pos2` (these positions corresponds to an invisible diagonal of a cuboid selection).
5. Create your schem file with `//mtschemcreate <name of your schem file>`. The file will be created into your “`worlds/<my_world>/schems`” folder.

- To **import a schem file** :

1. Enter in your world (with WorldEdit activated).
2. Grant yourself all privileges: `/grantme all`
3. Put a schem file into your “`worlds/<name of your new world>/schems`" folder.
4. Press F5 to show the coordinates.
5. Place a position where you want with command: `//pos1`
6. Import your schem file with `//mtschemplace <name of your schem file>`

## See also

- [Minetestmapper](/for-server-hosts/minetestmapper), a program to draw a 2D map of a Luanti world.
