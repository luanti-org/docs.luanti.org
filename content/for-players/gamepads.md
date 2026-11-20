---
title: Gamepads
aliases:
  - /Gamepads
  - /gamepads
---

Many players prefer using gamepads (aka controllers or joysticks) over a keyboard and mouse.

## Built-in support

### Version previous to 5.17

Luanti has some experimental built-in support for gamepads but it is quite broken and doesn't work on most hardware. Even when it does work, it doesn't support using GUIs.
**it's recommended that you use an external program for controllers, and not Luanti**.

### Version 5.17 and greater

Luanti has the start of gamepad support. You can use it to move and interact with the world, but not to use menus/GUIs. We recommend using it with another form of input for menus (like touchscreen or a mouse) or on games that do not involve menus. We plan on improving support in the future.

## External programs

### AntiMicroX

[AntiMicroX](https://github.com/AntiMicroX/antimicrox/) is a graphical program used to map gamepad keys to keyboard, mouse, scripts and macros. You can use this program to control any desktop application with a gamepad on Linux and Windows.

Todo: tutorial

### Steam Input

You can bind your controller to Luanti controls using Steam Input

#### On Desktop

[![](/images/gamepads/300px-Screenshot_20221217_110713.png)](/images/gamepads/Screenshot_20221217_110713.png)

1.  Install and open Steam
2.  Library > Add a game > Add a Non-Steam Game > Find and select Luanti > Add selected programs

    - If Luanti isn't in the list, click "Browse" and select the executable

3.  Open Big Picture Mode (click the expand icon in the top right of the Steam window, to the left of minimize/max/close)
4.  Go to Library > Luanti > Manage Shortcut > Controller Options
5.  Enable "Allow desktop configuration in launcher" and select "Forced On" in Steam Input Per-Game. Click OK
6.  Click "Controller Configuration"
7.  Click "Browse Configs" in the bottom of the Steam Controller Configurator

    - You can find community layouts in "Community"
    - OR you can use "Templates > Keyboard (WASD) and Mouse" to make your own

8.  Click Apply Configuration and then click Done
9.  Click "Your shortcut" and then "Play".

#### On Steam Deck

For a more in-depth guide, see [rubenwardy's blog post](https://blog.rubenwardy.com/2022/12/02/minetest-steam-deck/#controls)

1.  Install Luanti
    1.  Power > Switch to Desktop Mode
    1.  Open Discover
    1.  Search for Luanti and click Install
    1.  When installed, go to Start > Games. Right-click Luanti (using left trigger) and click "add to Steam"
    1.  Desktop > Return to Game Mode
1.  Library > Non-steam Games > Luanti
1.  Select controller icon, and choose a layout:

    - It's recommended that you use Community > Semi-official Steam Deck by rubenwardy
    - OR you can use "Templates > Keyboard (WASD) and Mouse" to make your own

1.  Play Luanti

### Others

- QJoyPad
- Joy2Key
