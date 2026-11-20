---
title: Creating Texture Packs
aliases:
  - /Creating_texture_packs
  - /creating-texture-packs
  - /content-dev/creating-texture-packs
---

[Texture packs](/for-players/texture-packs) are a simple folder containing image files named after the default ones. Luanti has very few requirements for using a texture pack and texture packs are normally used by games.

## Image Structure

Below is a brief overview of the texture structure as of version 5.1

### Luanti

The Luanti engine has very few base textures; these can be found under `your/path/to/luanti/textures/base/pack`. This includes textures for things such as; server icons, minimap overlays, logos and headers.

### Game Specific Textures

Each game that you install to Luanti will have different texture requirements due to the mods that games creator will have used to create the game. For example, Minetest Game includes 31 mods as of ver 5.1, some of these mods will have texture requirements and some will not.
For example `your/path/to/luanti/games/minetest_game/mods/default/textures` has an extensive array of textures to support this mod, whereas `your/path/to/luanti/games/minetest_game/mods/spawn` has no textures to support the mod.

### Engine and Game Texture Pack

In order to create a fully custom texture pack for a game, say Minetest Game, you would need to first copy all the textures that are under `~/base/pack` to a new folder. Then, copy all the textures from each individual `~/minetest_game/mods/modname/textures` to the same folder. This would give you a full list of the image files to create a custom texture pack for that game.

## Setting up Custom Texture Pack Folder

The Luanti engine supports sub-folders under the custom texture pack folder. Although these sub-folders can be named anything, it is sometimes easiest to keep the subfolders the same name as the mod the textures are for. This can also help with identifying what textures might be missing if you wish to support multiple different games with your custom texture pack for example Minetest Game.

### Create Texture Pack Folder

Under `your/path/to/luanti/textures` create a new folder that matches the name for your new texture pack for example: `Your_Texture_Pack_Name`

There is a small text file named `texture_packs_here.txt` in the correct location.

### Copy Textures Over - Simple

Windows:

- Browse to your Luanti folder
- Enter the `games/minetest_game` folder (or whatever game you want to make a texture pack for)
- Then hit Ctrl+F to invoke the search function,
- In the search box, enter `*.png` to find all the image files (or directly use the “Images” option in the search page),
- Next Select All Ctrl+A,
- Copy Ctrl+C and
- Paste to your texture pack folder Ctrl+V

If you would also like to replace some of the base engine textures:

- Browse to your Luanti folder,
- Enter the `textures/base/pack` folder,
- Select the files you wish to copy Ctrl + mouse click you might like to change:
  - `logo.png` - changes the ICON on the lower part of the main screen for your game
  - `minimap_*.png` - changes the appearance of the minimap in game
  - `unknown_*.png` - changes the appearance of unknown game objects
  - `server_*.png` - change all the server icons
  - `progress_*.png` - changes the load bar when a game loads
- Copy Ctrl+C and
- Paste to your texture pack folder Ctrl+V

Linux:

It’s better to use the terminal. These commands should do the job:

```bash
TP_NAME=your-pack-name
cd your/path/to/luanti
mkdir textures/$TP_NAME
find games/minetest_game/ -name '*.png' -exec cp '{}' "textures/$TP_NAME" ';'
```

If you would also like to replace some of the base engine textures:

- Browse to your Luanti folder,
- Enter the `textures/base/pack` folder,
- Select the files you wish to copy - you might like to change:
  - `logo.png` - changes the ICON on the lower part of the main screen for your game
  - `minimap_*.png` - changes the appearance of the minimap in game
  - `unknown_*.png` - changes the appearance of unknown game objects
  - `server_*.png` - change all the server icons
  - `progress_*.png` - changes the progress bar when a game loads
- Copy and Paste to your texture pack folder

Both methods will result in all textures appearing in single big list of textures/image files under the base custom texture pack folder.

### Copy Textures Over - Complex

The below methods will mean you end up with each texture under a folder named the same as the mod it is required for.

Windows:

To copy over using Windows it's easiest to make use of `robocopy` at the windows CMD prompt as this can easily copy over just `*.png` files while retaining the mod folder structure.

- Make a note of the source location for the files and the destination location eg
  - Source: `your/path/to/luanti/games/minetest_game/mods`
  - Destination: `your/path/to/luanti/textures/Your_Texture_Pack_Name`
- Open the [Windows command prompt](https://www.howtogeek.com/235101/10-ways-to-open-the-command-prompt-in-windows-10/)
- Type in:

```bat
robocopy "C:\your\path\to\luanti\games\minetest_game\mods" "C:\your\path\to\luanti\textures\Your_Texture_Pack_Name" *.png /s
```

If you would also like to copy over the default engine graphics using robocopy, then type in:

```bat
robocopy "C:\your\path\to\luanti\textures\base" "C:\your\path\to\luanti\textures\Your_Texture_Pack_Name" *.png /s
```

Under your texture pack folder, you should end up with a structure resembling:

| C:\your\path\to\luanti\textures\Your_Texture_Pack_Name\| |                                 |                        |
| -------------------------------------------------------- | ------------------------------- | ---------------------- | ---- |
|                                                          | default\|textures\|\*.png files |
|                                                          | doors\                          | textures\|\*.png files |
|                                                          | ....                            | ....                   | .... |

The texture folder is buried one extra level deep, but you can manually go through and simply use Windows file explorer to move the files up one level. Alternatively, you can move the files using the following Batch script:

This method requires the creation of a `.bat` file and that this file is placed and run from your new texture pack folder.

- Copy the below into a new text document:

{{% comment %}} cspell:disable {{% /comment %}}

```bat
@echo off
set thisdir=%cd%
    for /f "delims=" %%B in ('dir /b /ad') do (
        echo Level 2 Directory: %%~dpnB
        cd /d "%%~dpnB"
        for /f "delims=" %%C in ('dir /b /ad') do (
          echo Level 3 Directory: %%~dpnC
          cd /d "%%~dpnC"
          move *.png ..\
          cd..
          rd "%%~dpnC"
        )
    cd..
    )
cd /d "%thisdir%
```

{{% comment %}} cspell:enable {{% /comment %}}

- Save this file as `something.bat` at the location `your/path/to/luanti/textures/Your_texture_Pack_Name`
- Double click the `.bat` file to run it. A command prompt window will flick up and disappear

Your file structure should now appear as:

| C:\your\path\to\luanti\textures\Your_Texture_Pack_Name\| |                       |
| -------------------------------------------------------- | --------------------- | ------------ |
|                                                          | default\|\*.png files |
|                                                          | doors\                | \*.png files |
|                                                          | ....                  | ....         |

Linux:

Hopefully if you're using Linux you'll know how to do something similar. Otherwise please [open an issue with the docs team](https://github.com/luanti-org/docs.luanti.org/issues/new) and we'll update this section :)

### Special Textures

Luanti has some "special" textures which can be provided by texture packs to re-texture engine-provided or special textures. See [texture_packs.md](https://github.com/luanti-org/luanti/blob/master/doc/texture_packs.md) in the Luanti source tree contains a list of all these special textures

## Editing or Creating Textures

### Copyright

Copyright is a tricky issue especially when it comes to images and graphics, most countries in the world consider the creator of the art piece to have the copyright on that piece of work for there lifetime + 70/100 years after death (there are exceptions to this in some countries). Additionally there is a significant amount of mis-information available on the web in regards to copyright and fair use. It's important to have an awareness of copyright as if your not careful it may preclude you from being able to share your texture pack with the Luanti community. For a start see [Wikipedia Copyright](https://en.wikipedia.org/wiki/Copyright).

Most importantly for some, taking copyrighted textures from other voxel games is strictly not allowed, and uploading this to ContentDB will result in immediate rejection. If you use textures from somewhere else, **make sure** that they are freely licensed (usually under some Creative Commons license) and follow the licensing terms (giving attribution is also common courtesy even if licensed under CC-0, it helps to find the source of textures you have used).

### Image Editing Software

In order to edit textures, you'll need some kind of image editing software. Any image editor that supports transparency should be more than enough, but if you want software recommendations these are the image editors most commonly used by the Luanti community.

- [GIMP](https://www.gimp.org/) - Fully featured FOSS image editor tool. Might be overwhelming for a new user, however is very powerful if learned.
- [Libresprite](https://libresprite.github.io/) - FOSS fork of Aseprite, specially designed for pixel art.
- [Paint.NET](https://paint.net/download.html) - Windows-only image editor, like MS Paint but slightly more featured (supports e.g. transparency).
- [Lospec's pixel software list with more options](https://lospec.com/pixel-art-software-list/)

### Creating Textures

Textures for Luanti maybe any size, but square images whose size is a power of 2 work best. For example 16×16, 32×32, 64×64, 128×128, 256x256 and 512x512 are preferred. You could go larger than 512 if you like but you'll be placing additional processing burden onto computing hardware for limited value in regards to graphics clarity. Some excellent highly detailed texture packs run in the range 128 to 256.

Generally Luanti games and mods use the `.png` image format, but `.jpg`, `.bmp` `.tga` are also supported and may exist in some mods.

Also check out [texture_packs.md](https://github.com/luanti-org/luanti/blob/master/doc/texture_packs.md) in the Luanti source tree.

#### Where to Start

Creating a texture pack is no small task with 100's of different mods out there. To provide an idea, [HDX](https://gitlab.com/VanessaE/hdx-128/tree/master) by Vanessa is a large texture pack 2553 unique texture files. However not all games use all mods so you can get excellent coverage of mods with a much smaller number of textures.

The biggest factor in completing a texture pack is that you set yourself a reasonable goal work on it slowly and enjoy what you're doing, following are some quick ideas of how/were to start.

##### Create an Alternate Texture Pack

To get your feet wet and see if you even enjoy creating textures, it's often easiest to create an alternate texture pack for one of the smaller mods. Maybe a new graphic for an existing mod that you feel fits in better with the basic Luanti texture pack. This can give you an idea or of what's needed and how long creating a texture pack will take. You can also wrap up the pack fairly quickly and get it out there for people to use.

##### Focus on default

The mod "default" is the most common mod used across most games as it includes all the basic textures to even generate a voxel map. That's not to say some games don't use it, just that many do. So if you can cover default your going to make some serious impact on the look and feel of a game. There are about 250 unique files inside default, the toughest textures to create are the animated ones eg torch and water. the easier tend to be wood planks and trees as often these can be a simple color shift in the first instance to differentiate tree and wood type.

Now you are all done and ready to share your texture pack. There's a few things to check before you do:

- Make sure to include a `license.txt` file and any relevant copyright and attribution notices in there.
- Make a good screenshot of your textures in use in game, as players will usually want to see the texture pack in action before installing.
- Make a `texture_pack.conf` file, which contains metadata about the texture pack. An example texture_pack.conf looks something like this:

```conf
name = very_cool_texture_pack
title = Very Cool Texture Pack
description = This is a description for the Very Cool Texture pack.
```

### Git hosts

There are numerous places that run and use [Git](https://git-scm.com/) as a place for hosting your files online. The beauty of Git is it gives you version control and allows other users of Luanti to download and potentially add to your work in the future. Git is probably the best option for sharing texture packs however it can be a bit challenging to set up initially. Popular providers include:

- [GitHub](https://github.com/)
- [Codeberg](https://codeberg.org/)
- [Gitlab](https://about.gitlab.com/)
- [and many more...](https://en.wikipedia.org/wiki/Comparison_of_source-code-hosting_facilities)

### ContentDB

[ContentDB](/about/contentdb) is an official source to host and distribute Luanti texture packs, games, and mods. It can be easily accessed and installed from the Luanti main menu.
