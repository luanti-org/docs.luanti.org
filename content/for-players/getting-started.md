---
title: Getting Started
aliases:
  - /Getting_Started
  - /getting-started
---

# Getting Started

Welcome to Luanti! This page explains what Luanti is all about, how to get it, and how to start playing your first games alone or online.

## “What is this strange ‘Luanti’ thing I keep hearing about?”

- **Luanti** is a platform on which you can play many **games** which are set in a **world entirely made out of blocks**, voxels.
- You can play **offline or online**, in singleplayer or multiplayer.
- Most games (but not all!) have a sandbox gameplay focused on construction, mining and creativity.
- You can browse the list of games available on [ContentDB](https://content.luanti.org/packages/?type=game).
- You can install **[mods](/for-players/mods/) to modify** certain **aspects of gameplay**. Mods are an **inherent** part of Luanti, it is even what games consist of.
- **Mods work out of the box when playing on [servers](/for-players/servers/)**, no additional installations required. Mods are server-side, everything is handled automatically.

## Getting Luanti

### Windows

- [Download Luanti](https://www.luanti.org/downloads/).
  - The 64-bit build is extremely recommended. Always use this unless you are absolutely sure you're on an old machine that doesn't have a 64-bit processor.
- Luanti on Windows is distributed in a portable archive. Extract it as a whole where you want, whether it be on your desktop or in your documents folder.
  - Keep in mind you need write permissions to the folder. **Do NOT save to `C:\Program Files\` or similar, as it will cause problems (no write access).**
- To run Luanti, open the extracted directory and look for the `bin` (binary) directory. Inside the `bin` directory is the Luanti executable, `luanti.exe`.
  - If you want a desktop shortcut or the like, just make create a shortcut to this executable.

If you prefer to use a package manager, you can install [Luanti as an MSYS2 package](https://packages.msys2.org/base/mingw-w64-luanti) (both `luanti.exe` and `luantiserver.exe` are available).

### macOS

- [Download Luanti for macOS](http://www.luanti.org/downloads/). You can pick the official .app, Homebrew or the MacPorts version.

### Linux

If you are on a distribution with up to date enough repositories (e.g. Arch), you can install Luanti from your package manager.

Otherwise it is recommended to obtain Luanti through other means, such as the [Flatpak package](https://flathub.org/apps/details/net.minetest.Minetest). For Ubuntu and similar, there is the [Luanti PPA](https://launchpad.net/~minetestdevs/+archive/ubuntu/stable).

If all else fails, you can build from source by [cloning the engine repository](https://github.com/luanti-org/luanti) and [following the build instructions](https://github.com/luanti-org/luanti/blob/master/doc/compiling/linux.md).

## Playing

Now that you have it installed you can either; play singleplayer, play on a local server or play online by connecting to a server.

### Play Singleplayer

When first booting up Luanti, it will ask you to install a game. The button that shows up will direct you to the content browser which will list the games that are available. You can also browse the available games [in your browser](https://content.luanti.org/packages/?type=game).

To install mods or texture packs, go to the "Content" tab, press "Browse Online Content" and pick the type of package you want to browse. To enable a texture pack select it in the content tab, and to enable mods you have installed, you need to select the world you want to play with mods in, press "Select mods", and enable the wanted mods.

For an interactive introduction to common Luanti gameplay elements, there is the [Tutorial](https://content.luanti.org/packages/Wuzzy/tutorial/) game.

### Play Online

Joining multiplayer server is done in the "Join game" tab. To select a server, click on its name in the server list. If you know of a server from e.g. a friend that's not listed in the server list, you can manually input the address and port to the right. You do not need to install any mods to play on a server, everything provided by the server is sent to the client during connect!

When you are new to a server, you need to first register. Press the register button and input the username and password you want to use. Next time you log in with this username and password.

**Accounts in Luanti are not centralised.** This means that accounts are stored on each server, rather than on a central server. You do not have to use the same username, but using different passwords are recommended (however servers cannot see your password, it is one-way hashed during transit). Some servers allow a blank password but using a password is strongly recommended to stop others stealing your player and causing damage. You can change your password by clicking 'change password' on pause menu (ESC).

### Basic Controls

_Most of these can be changed in the “Change Keys” menu. For a more complete list of keyboard controls, see [Controls](/for-players/controls/)._

The default and most important controls are:

- **W/A/S/D**: move
- **Space**: jump
- **Left mouse button**: Punch, mine nodes, move an item stack in an inventory
- **Right mouse button**: Use (e.g. open chests or furnaces), place nodes, move one item or split items in an inventory
- **Shift**+**Right mouse button**: place blocks
- **Middle mouse button**: move 10 items in an inventory
- **Mouse wheel**: select item in the hotbar
- **0**-**9**: select item in the hotbar
- **Q**: drop block, item or tool in hand
- **I**: open or close the inventory menu
- **T**: open the chat window
- **Shift**: descend on ladders or sneak (walk slower, prevents falling off ledges)
