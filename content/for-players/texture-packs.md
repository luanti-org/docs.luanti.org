---
title: Texture Packs
aliases:
  - /Texture_Packs
  - /texture-packs
---

A **texture pack** is a collection of files that are used to change the in-game textures of:

- [blocks](/for-players/nodes)
- [items](/for-players/items)
- [mobs](/for-players/mobs)
- GUI

The native resolution of Luanti's textures are 16 × 16 pixels.

All versions of Luanti support custom textures with a folder under the main Luanti folder called _textures_. This folder was added in 0.4.dev-20120408 _(18d8e3ac)_. Changing texture packs is done via the in game main menu tab _Content_.

## Installation

Since version 5.0, texture packs available in [ContentDB](/about/contentdb) are available in the Content tab -> Browse online content button. Texture packs downloaded in such a way are automatically installed.

In any version of Luanti, the way to install a custom texture pack is by doing the following:

1.  Download a texture pack from somewhere.
2.  Extract the texture pack into the _textures_ directory in your Luanti directory. If on Windows, this folder should have a _texture_packs_here.txt_ file inside. Use a program such as 7-zip to extract the compressed folder, if you cannot open the texture pack archive.
3.  Once your texture pack is copied into the _textures_ folder start/restart Luanti.
4.  Select the _Content_ tab at the top of the main Luanti menu. Texture packs will appear in green in the left hand view pane.
5.  Select the texture pack you wish to use using the mouse.
6.  Double-click or select the _Use Texture Pack_ button on the lower right of the window.
7.  The word _Enabled_ should appear next to the texture pack you selected.

## Texture Packs

ContentDB has a section dedicated to texture packs. Pick the resolution you would want:

| Image                                              | Resolution                                                                                                                     |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| ![](/images/texture-packs/Texture_block_4_8.png)   | [Low resolution texture packs](https://content.luanti.org/packages/?type=txp&page=1&tag=less_than_px) (<16 pixels)             |
| ![](/images/texture-packs/Texture_block_16.png)    | [Normal resolution texture packs](https://content.luanti.org/packages/?type=txp&page=1&tag=16px) (16 pixels - same as default) |
| ![](/images/texture-packs/Texture_block_32_64.png) | [High resolution texture packs](https://content.luanti.org/packages/?type=txp&page=1&tag=32px) (32 pixels)                     |
| ![](/images/texture-packs/Texture_block_32_64.png) | [Very high resolution texture packs](https://content.luanti.org/packages/?type=txp&page=1&tag=64px) (64 pixels)                |
| ![](/images/texture-packs/Texture_block_128.png)   | [Extremely high resolution texture packs](https://content.luanti.org/packages/?type=txp&page=1&tag=128px) (128+ pixels)        |

There is also the [Texture Packs forum](http://forum.luanti.org/viewforum.php?f=4) in the [Luanti Forums](http://forum.luanti.org/)

_Note that as the pixel count of a texture pack increases a more powerful device will be needed to render and run Luanti._

## Texture Pack Creation

To create a custom texture pack, you must edit the default files. If you have experience with image editors then creating your own custom texture pack is fairly straightforward.

1.  Locate some source textures to modify. You can grab the original textures from mods by going into their 'textures' folder, or all the original textures for [Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/) from the 'textures' folders within the various 'mods' it contains.
2.  Create a new folder to hold your new texture pack.
3.  Use your preferred image editing program – [GIMP](http://gimp.org/) is free/open-source and works well – and create a PNG file for each texture that you want to modify. Any image editor that supports transparency – also called “alpha” – should be OK.
4.  The textures may be any size, but square images whose edge lengths are powers of 2 (16 × 16, 32 × 32, 64 ×64, 128 × 128, …) are preferred for visual and consistency reasons.
5.  Compress the folder — not only the files inside – in a .zip archive. You can then upload it onto [ContentDB](https://content.luanti.org/packages/new/) and/or post it in the [Texture Packs forum](https://forum.luanti.org/viewforum.php?id=4).

For more in depth instructions, see [Creating Texture Packs](/Creating_texture_packs).
