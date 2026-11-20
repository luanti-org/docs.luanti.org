---
title: Distributing Games Outside ContentDB
aliases:
  - /distributing-luanti-games
---

This page documents how to publish a Luanti game outside of [ContentDB](/about/contentdb). It assumes that you know how to compile Luanti from source.

Luanti as an engine itself doesn't have any official means to export a game into a self-contained package with the engine, instead games are published on [ContentDB](https://content.luanti.org/packages/?type=game) which allows your game to be installed from the main menu content browser. However as Luanti is free software you can fork the engine to rebrand it and bundle it with your game for distribution outside of the Luanti ecosystem.

_(This page is somewhat outdated, but still contains useful information that can be reapplied for the latest version of Luanti)_

## General tips

Generally you're able to customize the main menu (at `builtin/mainmenu/`), which is written in Lua and formspec, however you'd like to suit your game.

### Locking down the singleplayer tab to one game

As of 5.8.0-dev, the engine will automatically detect what games are installed and pick the first one installed. If you ship the engine with just your game, then it will select that game on the main menu.

To remove the game bar, it's enough to just remove this code in `builtin/mainmenu/tab_local.lua` and set the sizes of the button bar to 0:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
@@ -78,32 +78,8 @@ function singleplayer_refresh_gamebar()

 	local btnbar = buttonbar_create("game_button_bar",
 		game_buttonbar_button_handler,
-		{x=-0.3,y=5.9}, "horizontal", {x=12.4,y=1.15})
+		{x=-0.3,y=5.9}, "horizontal", {x=0,y=0})

-	for _, game in ipairs(pkgmgr.games) do
-		local btn_name = "game_btnbar_" .. game.id
-
-		local image = nil
-		local text = nil
-		local tooltip = core.formspec_escape(game.title)
-
-		if (game.menuicon_path or "") ~= "" then
-			image = core.formspec_escape(game.menuicon_path)
-		else
-			local part1 = game.id:sub(1,5)
-			local part2 = game.id:sub(6,10)
-			local part3 = game.id:sub(11)
-
-			text = part1 .. "\n" .. part2
-			if part3 ~= "" then
-				text = text .. "\n" .. part3
-			end
-		end
-		btnbar:add_button(btn_name, text, image, tooltip)
-	end
-
-	local plus_image = core.formspec_escape(defaulttexturedir .. "plus.png")
-	btnbar:add_button("game_open_cdb", "", plus_image, fgettext("Install games from ContentDB"))
 end
```

{{% comment %}} cspell:enable {{% /comment %}}

### Rebranding the main menu

Copy the game's `menu/` textures into `textures/base/pack/`.

- Rename `header.png` to `menu_header.png`
- Rename `background.png` to `menu_bg.png`
- Rename `icon.png` to `logo.png`

The main menu branding doesn't support randomized textures (unless you implement that yourself), so you would need to pick one.

### Add to the "About" tab

You should keep the Luanti credits list in order to follow Luanti's license regarding attribution, but you may choose to add contributors and developers to the game above it.

In addition to this, you should add some note right above the Luanti credits to credit the engine itself:

```lua
local minetest_info = {
	"<GAME> is made possible by the Luanti engine,",
	"which is free software licensed under LGPLv2.1.",
	""
}
```

### Filtering the serverlist

You can filter the serverlist to only show servers running the specified game. Add the following code to `builtin/mainmenu/serverlistmgr.lua`:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
@@ -181,7 +181,18 @@ function serverlistmgr.sync()
 			end

 			local retval = core.parse_json(response.data)
-			return retval and retval.list or {}
+
+			local list = {}
+			local kiosk_game = "<gameid of the game>"
+
+			-- Iterate over servers, only keep ones with the valid gameid
+			for _,entry in pairs(retval.list) do
+				if entry.gameid == kiosk_game then
+					table.insert(list, entry)
+				end
+			end
+
+			return list
 		end,
 		nil,
 		function(result)
```

{{% comment %}} cspell:enable {{% /comment %}}

Replace the `kiosk_game` variable with the gameid of your game.

### Filtering ContentDB packages

ContentDB keeps track of what games a mod is able to support, which can be used to filter the list of packages through the API. This works especially well if the game has a distinct mod ecosystem. Keep in mind that the game needs to be published onto ContentDB for this to work, otherwise you may choose to maintain your own ContentDB instance with a curated set of packages that work with the game.

In `builtin/mainmenu/dlg_contentstore.lua`, where it constructs an API URL that looks something like this:

```
"/api/packages/?type=mod&type=game&type=txp&protocol_version=" ..
```

Add the `&game=<author>/<game_package>` argument onto the URL. For example, `Warr1024/nodecore` for NodeCore.

Mods and texture packs are able to specify game compatibility, however game agnostic mods will unfortunately not be shown.

### Change default settings

You can change the default settings in `src/defaultsettings.cpp` to better suit your game. You can also tweak settings for particular platforms, for example the Android specific default settings which are inside of a `#ifdef __ANDROID__` preprocessor directive.

## Windows

### Rebranding

To fully change the name of the engine you can change the name of the "project" in `CMakeLists.txt`. This will change the name of the executable and the name used on the title bar, among other places. Keep in mind this will also mean you need to rename files in `misc/` and the translation files.

To change the icon that's used for Luanti builds on Windows you'll have to replace `misc/minetest-icon.ico` with your game's icon in the Windows ICO file format.

To make it install your given game instead of Minetest Game, go into `CMakeLists.txt` and change the line that installs `mods/minetest_game` to install your game instead. You may also remove things such as the documentation (e.g. lua_api.txt) and devtest. The lines that install this is right around where the game gets installed, just remove those.

## Android

This section assumes you already know how to build the Android version of Luanti and how to sign APK files. (TL:DR - Download the Android command-line tools, install the NDK and accept any necessary license terms, then `./gradlew assemblerelease`, `apksigner` and be happy)

### Separate package name

First of all you would like to change the package name of the Android app. This will make it separate from the regular Luanti app which has the package name `net.minetest.minetest`. Change the `applicationId` in `android/app/build.gradle` to the package name of your choosing. Package names work like regular Java names which use reverse DNS names, if you don't have a domain name it should be fine to keep `net.minetest` and only change the last "subdomain" to the game's name.

However if you try to build and install the app right now it will not work. This is because Luanti provides a file provider which needs to be named just like the package name, and will conflict with the regular Luanti app if you have that installed. In `android/app/src/main/AndroidManifest.xml` and `android/app/src/main/java/net/minetest/minetest/GameActivity.java` you will have to change the file provider name to reflect your package name.

### Rebranding

To change the name of the app to the game's name, edit the `label` string in `android/app/src/main/res/values/strings.xml`. To change the icon, replace `android/app/src/main/res/mipmap/ic_launcher.png` with the icon of the game.

When Gradle bundles assets into an archive, it will copy `gameToCopy` defined in `android/app/build.gradle`. By default this is `minetest_game` so you would want to change the variable to bundle your game instead.

The loading screen background when Luanti extracts assets can be customized too. `android/app/src/main/res/drawable/background.png` which by default is a basic light blue background will tile if you replace it with something else.

## Linux

Linux users are usually more advanced, most of the time they will compile the engine from source for their particular distro and package it for you (the Linux user is a wonderful creature). To give them better desktop integration you should edit `misc/net.minetest.minetest.desktop` which will give them a application launcher shortcut that accurately reflects the game, including the proper icon.

## macOS

TODO

## Licensing

As the Luanti engine is licensed under LGPLv2.1, it means that any changes you make to the engine source code need to be made available. This however may not apply to the game you're distributing, which could even be licensed under proprietary terms if you do not make use of any copyleft mods. This isn't legal advice however, and you should consult a lawyer to be absolutely sure.

You can choose to version control your engine fork in Git (which has the added bonus of being able to automatically make builds using CI on your favorite Git host). Alternatively for the absolute minimum necessary you can export your current source tree with modifications as an archive file using something like this:

```bash
stashName=`git stash create`; git archive $stashName | gzip > ../cool-game-engine-fork-$(date +'%Y-%m-%d').tar.gz
```

Make some place where this file is available, and be sure to update it whenever new builds are made. `gzip` can be replaced with your favorite compression method, if so desired.

## Examples

A large portion of the initial version of this page documents changes that were made to the engine for the [NodeCore Android app](https://play.google.com/store/apps/details?id=se.voxelmanip.nodecore). Its source code is available for download [here](https://nodecore.voxelmanip.se/src/) and can be used as an example of how to use the advice given on this page (though it is several versions behind at this point).
