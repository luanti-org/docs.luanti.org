---
title: Servers
aliases:
  - /Server
  - /server
---

Luanti **servers** allow players to play online with other people. They can be run from a dedicated server, a Virtual Private Server or a home computer.

This is not a list of servers, please see the [Luanti Server List](https://luanti.org/servers) or [Luanti Forum servers section](https://forum.luanti.org/viewforum.php?f=10) instead.

To setup your own server, see [For Server Hosts](/for-server-hosts)

## Getting access to a server

### Finding a server

![Luanti client's server list](/images/server/Minetest_serverlist.png)

In order to play on a server at all, you need to know the address and a port number of a server first. There are many ways to find those addresses:

- **Server list in Luanti**: The easiest way to obtain a list of servers found within Luanti itself. You find it in the main menu under the tab “Join Game”.
- **Server list website**: **[https://www.luanti.org/servers/](https://www.luanti.org/servers/)** has the same server list as above, but you can view in your browser.
- **Luanti Forums**: There is a subforum called “[Servers](https://forum.luanti.org/viewforum.php?f=10)” entirely devoted to servers.
- **Friends**: If you know a friend who hosts a Luanti server, ask for the address and port number.

### Connecting to a server

If you have obtained address and the port number of a server, you just have to enter those values into the respective fields in the “Join Game” tab in the main menu. In case you used the in-game server list, Luanti automatically enters those values for you.

#### Account registration

If you are new to a server, you need to register on that server. Luanti does not have centralised authentication, so you will need to register for every server you play on. Press the _Register_ button and type in the username and password you want to use.

Player names have the following limitations:

- Allowed characters are a-z, A-Z, 0-9, the hyphen (“-”) and the underscore (“\_”)
- The name must not be “singleplayer”
- The name must have a length of 1-20 characters

The next time you log into the server, you will input the username and password you registered with and press the _Login_ button.

**Warning**: There is also no automatic mechanism to recover a lost password. If you lost your password, it's tough luck for you. You could try to contact one of the server operators, but there is no guarantee they'll help you.

#### Logging in

The next time you log into the server, you will input the username and password you registered with and press the _Login_ button.

### Basics

The gameplay in a multiplayer server is basically the same as in a singleplayer game. The same rules apply. See [Getting Started](/for-players/getting-started#gameplay) for gameplay-related concepts. Well, at least in theory.

In practice, every server is different. They could just host the vanilla Minetest Game, or host Minetest Game with many crazy mods installed, or even host an entirely different game. Be prepared to be surprised! :D To see the game a server is running, you can check the **gameid** in the server list, and to see the modset it is running you can type `/mods` if it hasn't been disabled. If the server administrator is nice they may provide the game with modset for download somewhere.

Also, different servers are usually managed by different people, they also may or may not have rules which may or may not be enforced.

### Useful things to know

In multiplayer servers, these things become more important:

- [Chat](/for-players/chat): learn how to communicate with other players
- [Privileges](/for-players/privileges): learn what you can and can’t do on a server
- [Server commands](/for-players/server-commands): learn how to use server commands. They are also sometimes useful for players; for example, you can pulverize an item, teleport (if you are allowed to), and more
  - Find out more about the server with commands like “/mods”, “/privs”, “/status”.
- The [mods](/for-players/mods) installed on the server
  - Take note of [mobs](/for-players/mobs), PvP (fighting “player-vs-player”), and server rules
- Custom settings which may affect gameplay
- When you press Esc, the game will not be paused like in single player mode

Also, you can generally connect to all servers, no matter how many mods they use or what game they host. All data (textures, sound, item/node definitions...) you need is downloaded automatically for you, you do not need to have the mods locally installed to be able to join a server running those mods.

### Griefing and protection

“Griefing” and “protection” are two words you will read frequently when playing on servers.

Griefing refers to the act of destroying, damaging, or vandalizing a building built by other players against their will. On some servers griefing is forbidden, on some servers it is allowed.

Protection mods are very common on servers. A protection mod is a mod which grants players ownership to certain parts of the world. Only the owner can add or remove blocks in an owned area. Protection basically eliminates griefing.

## See also

- [For Server Hosts](/for-server-hosts)
