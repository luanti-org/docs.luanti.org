---
title: Server Commands
aliases:
  - /Server_commands
  - /server-commands
  - /server/commands
---

**Server commands** (also called “**chat commands**”) are special commands to the server that can be entered by any player via the [chat](/for-players/chat) to cause the server to do something.

## Command basics

Luanti comes with a set of built-in commands, but games and [mods](/for-players/mods) may add custom commands or even alter or remove some of the built-in-commands.

There are many commands, but you only need to remember the `/help` command. This will give you a list of all commands that are _currently_ available, plus a brief explanation.

There are a few commands which can be issued by everyone, but some commands only work if you have certain [privileges](/for-players/privileges) granted on the server. Use the command `/privs` to see your own privileges. If not noted otherwise, the commands in this article are assumed to require no privileges.

## Issuing a command

To issue a command, simply type it like a [chat](/for-players/chat) message or use the [console](/for-players/console). Alternatively, you can just press the “/” key (only in default [controls](/for-players/controls)) which simply opens a chat window where the “/” has already been typed for you and then type the command right away. The command itself will _not_ appear in the chat. Since every command starts with “/”, this means that ordinary chat messages can’t start with “/”; they will be interpreted as a command instead, even if such a command does not exist. You can tell whether or not a command was successful by the server’s response.
{{% comment %}} cspell:disable {{% /comment %}}
If you see something like “`-!- Invalid command: /blargh`” in the chat, you probably misspelled something.
{{% comment %}} cspell:enable {{% /comment %}}
The most commands will cause the server to write you something else on the chat log for you, if successful.

## Issuing a command from the system terminal

To issue commands on a Luanti server instance started from the terminal, Luanti has to be built with the [ncurses](https://www.gnu.org/software/ncurses/) library enabled. When `luantiserver` is started with the `--terminal` argument, commands can be executed as they are in-game; i.e. `/<command>`

## General syntax

All server commands start with “/”. After that, one word follows which is itself followed by some or none arguments. You’ll find the exact syntax in the command reference. In the following command reference, text enclosed in `<>` is a placeholder for an actual value. Anything written in `[]` can be omitted.

### Relative numbers (tilde notation)

Some commands that accept numbers can accept relative numbers. This means the value is relative to something else, like the current position. To use a relative value, prepend the number with a `~`. The number will then be added to the reference value. E.g. `~5` means the reference value plus 5, `~-7` means the reference value minus 7. Also, writing `~` on its own stands for the reference value. This is also known as the “tilde notation”.

Example: `/teleport ~ ~10 ~` teleports you 10 blocks above your current position.

## Command reference of built-in commands

The commands listed here are built into Luanti by default. They’re almost always available, but it’s possible for games and mods to remove even built-in commands (but this is rarely used).

This reference is up-to-date for version 5.9.0. Remember to use `/help` for the latest command reference (but it is less detailed).

### Quick documentation

Show short documentation of server commands and privileges.

- `/help`—Shows a list of the available commands on the server
- `/help <command>`—Shows short description about the given command
- `/help all`—Same as `/help all`
- `/help privs`—Lists all privileges on the server that could possibly be granted to players and shows a short description about each of them

With the exception of `/help <command>`, these commands open a window where you can browse the available commands. They are organized by origin. “\*builtin\*” contains commands that come with Luanti. The other sections contain commands coming from mods.

The text color indicates whether you have the necessary privileges for a command. A green command means you can use it, gray means you cannot. For `/help privs`, privileges in green are yours.

If you append `-t` to the command (with the space), the commands will be listed in the chat instead.

#### Informational

- `/privs [<player>]`—List of privileges granted to `<player>`, if not specified, your own privileges
- `/haspriv <privilege>`—List all online players that have the specified privilege
- `/last-login [<player>]`—Show the date and time when `<player>` has logged in the last time into this server ([UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) time zone, [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format). If not specified, shows your own last login time

#### Chat

These commands require the “shout” privilege to work.

- `/msg <player> <message>`—Send a direct message `<message>` to `<player>`; but not to the other players. **Note**: The message is not really secret. Anyone intercepting the network traffic and the server operator could still, in principle, read it
- `/me <action>`—Makes a text in the format “\* `<your name> <action>`” appear in the chat log. E.g. “/me eats pizza.” leads to “\* Alfred eats pizza.” (if your name is “Alfred”)

See [Chat](/for-players/chat) for details

#### Items

- `/give <player> <itemstring> [<count> [<wear>]]`—Give the specified item (see [itemstrings](/for-players/itemstrings)) `<count>` times (default: 1) to the player. `<wear>` specifies the damage for tools (0-65535) and is meaningless for other items, higher means more damage (default: 0). Requires the “give” privilege
- `/giveme <itemstring> [<count> [<wear>]]`—Give item to yourself. `<count>` and `<wear>` have the same meaning as for /give. Requires the “give” privilege.
- `/pulverize`—Destroys the wielded item. Can be used by any player
- `/clearinv [<name>]`—Destroys all items in your inventory (no argument provided) or in someone else's inventory (`name` provided). To clear someone else's inventory, you need the “server” privilege

**Hint**: Negative numbers for `<count>` and `<wear>` will count down from 65536, so you can use -1 as shorthand for 65535, the maximum possible value.

##### Examples

- `/giveme default:torch`—Gives you a torch
- `/give Peter default:cobble 50`—Gives Peter 50 cobblestone
- `/giveme default:pick_steel 1 16383`—Gives you a steel pickaxe which is about 25% worn-off

#### Teleportation

Teleportation is the immediate displacement of any player to a given position. All of the following commands require the “teleport” privilege:

- `/teleport <x>,<y>,<z>`—Teleports yourself to given [coordinates](/for-players/coordinates)
- `/teleport <target_player>`—Teleports yourself to the player with the name `<target\_player>`
- `/teleport <player> <x>,<y>,<z>`—Teleports `<player>` to given coordinates. Also requires the “bring” privilege
- `/teleport <player1> <player2>`—Teleports `<player1>` to `<player2>`. Also requires the “bring” privilege

The coordinates support relative values with `~` (see above). You can set a position relative to your current position then.

[Minetest Game](https://content.luanti.org/packages/Minetest/minetest_game/) also provides the command “`/home`”.

#### Kill

- `/kill [<name>]`: Kill player or yourself. Requires “`server`” privilege

### Moderation

#### Password manipulation

These commands allow to set and reset the passwords of any player and require the “password” privilege to work.

- `/setpassword <player> <password>`—Sets password of `<player>` to `<password>`
- `/clearpassword <player>`—Makes password of `<player>` empty

#### Privilege manipulation

All these commands require you to have the “privs” (to manipulate all privileges) or “basic_privs” \[to manipulate the privileges set as basic_privs in the minetest.conf ( default “interact” and “shout”) privileges\] privilege.

- `/grant <player> <privilege>`—Gives the `<privilege>` to `<player>`
- `/grant <player> all`—Give all available privileges to `<player>`
- `/grantme <privilege>`—Give `<privilege>` to yourself
- `/grantme all`—Gives all privilege to yourself
- `/revoke <player> <privilege>`—Takes away a `<privilege>` from `<player>`
- `/revoke <player> all`—Takes away as many privileges as possible from `<player>`
- `/revokeme <privilege>`—Takes away a `<privilege>` from yourself
- `/revokeme all`—Takes away as many privileges as possible from you

`<privilege>` can also take a list of privileges, each separated by a comma. For example: `/grantme fly,noclip,fast` grants you the “fly”, “noclip” and “fast” privileges.

#### Excluding players from server

These commands allow the user to kick, ban and unban players. Kicking a player means to remove a connected player from the server. This requires the “kick” privilege. Banning a player prevents him/her to connect to the server again. The player does not need to be connected at this time. Unbanning means to remove a ban from a player, allowing him/her to connect to the server again. The ban and unban commands require the “ban” privilege.

- `/kick <player name> [<reason>]` – Kicks the player with the name `<player name>`. Optionally a `<reason>` can be provided in text-form. This text is also shown to the kicked player.
- `/ban` - show list of banned players
- `/ban <player name>`—Ban IP of player
- `/unban <player name>`—Remove ban of player with the specified name
- `/unban <IP address>`—Remove ban of player with the specified IP address

#### Informational

Request some information from the server; the answer from the server will also be written into the chat log.

- `/admin`—Player name of the administrator / server operator of the server you're connected to.
- `/status`—Shows some information about the server. Usually, this is the server’s Luanti version, the uptime (time the server has been running without interruption), list of connected players and the message of the day (if it exists). If the uptime does not have a time unit, it is in seconds. Servers can customize this message.
- `/mods`—List of mods installed on the server.
- `/days`—Current game day (counting starts at 0)
- `/time`—Current game time (24h clock)

#### World manipulation

- `/time <hours>:<minutes>`—Sets the time of day in the 24-hour format (0:00-23:59). Requires the “settime” privilege. Precede the time with a tilde for a relative time change.
- `/time <time_of_day>`—Sets the time of day as a number between 0 and 24000. Requires the “settime” privilege. Supports relative number syntax with `~` (see above).
- `/set -n time_speed <speed>`—Sets the speed of day/night cycle where `<speed>` is the time speed (read as “`<speed>` times faster than in real life”). 72 is the default, which means a day-night cycle lasts 20 minutes by default. Requires the “server” privilege
- `/spawnentity <entity> [<X>,<Y>,<Z>]`—Spawns an [entity](/for-engine-devs/objects) of type `<entity>` near your position or at the X,Y,Z coordinates, if specified. Requires “give” and “interact” privileges. The coordinates support relative values with `~` (see above)

#### Server maintenance

All of these commands require the “server” privilege.

- `/shutdown [-r]`—Shuts down the server. If `-r` is provided, players are given an offer to reconnect
- `/shutdown <delay> [-r]`—Shuts down the server in `<delay>` seconds or aborts a pending shutdown if the number is -1. If `-r` is provided, players are given an offer to reconnect
- `/set <variable>`—Shows the value of the given server `<variable>` (see [minetest.conf](/for-players/minetest-conf) for a list of variables)
- `/set <variable> <new value>`—Sets the existing server `<variable>` to the given `<new value>`
- `/set -n <variable> <initial value>`—Creates a new server variable named `<variable>` and sets it to `<initial value>`
- `/clearobjects [full|quick]`—Clears objects/entities (removes dropped [items](/for-players/items), [mobs](/for-players/mobs) and possibly more) on the server. In “quick” mode (default), objects in loaded mapblocks are removed immediately, while other objects are removed when the mapblock they're in is loaded. In “full” mode, all objects are cleared. Quick mode is very fast, but the full mode may slow down the server to a crawl for 10 to more than 60 seconds or even freeze it.
- `/auth_reload`—Reloads _auth.txt_, which is the authentication data, containing privileges and Base64-scrambled passwords
- `/emergeblocks here [<radius>]`—Starts loading (or generating, if inexistent) map blocks around the player's current position with an optional radius (in nodes)
- `/emergeblocks <pos1> <pos2>`—Starts loading (or generating, if inexistent) map blocks contained in the area within pos1 and pos2
- `/fixlight here [<radius>]`—Resets lighting around the player's current position with an optional radius (in nodes)
- `/fixlight <pos1> <pos2>`—Resets lighting contained in the area within pos1 and pos2
- `/deleteblocks here [<radius>]`—Removes the MapBlock the player is in, from the database. As this triggers mapgen, this might start mechanisms like mud reflow or cavegen which very likely affect mapblocks outside the specified range. 113 blocks are a safe-distance for a server with no interfering mods. `<radius>` is an optional argument to specify the range (in nodes) in which MapBlocks are deleted
- `/deleteblocks <pos1> <pos2>`—Removes the MapBlock containing blocks inside the area from pos1 to pos2 from the database. May crash for larger areas. Warnings from above apply
- `/remove_player <name>`—Removes all data associated with the given player. This only works if the player is currently not connected. This is not deleting authentication information. If a player with this name connects again, he/she inventory, position, etc. are gone. Password remains.

Note: The coordinates in `/emergeblocks`, `/fixlight` and `/deleteblocks` support relative numbers (relative to your current position) with `~` (see above).

### Rollback

Requires the “rollback” privilege.

- `/rollback_check [<range>] [<seconds>]`—Checks who has last touched a node or near it, max. `<seconds>` ago (default `<range>=0`, default `<seconds>=86400`, which equals 24 hours in real time).
- `/rollback <player name> [<seconds>]`—Reverts actions of a player; default for `<seconds>` is 60
- `/rollback :<actor name> [<seconds>]`—Reverts actions of an actor _(not a player)_; default for `<seconds>` is 60

### Profiler

The `/profiler` command is a hidden command for developers to test the performance of a game or mod. It is only available if the setting `profiler.load` is enabled.

If enabled, the profiler starts when the server is launched and then constantly runs in the background and measures the time of certain functions.

- `/profiler print [<filter>]`: Prints profiler data into chat (hint: open chat console for better readability)
- `/profiler dump [<filter>]`: Writes profiler data Luanti log
- `/profiler save [<format> [<filter>]]`: Save profiler data into a file in the world directory in a given file format. Possible formats are: txt, csv, lua, json, json_pretty
- `/profiler reset`: Resets the collected profiler data

The `<filter>` argument optionally filters the output by modname.

## Command reference for Minetest Game commands

If you use Minetest Game, a few additional commands are available. These commands may not be available if you use a different games.

- `/sethome`—Sets your current position as your “home point”. Requires the “home” privilege
- `/home`—Teleports yourself to your “home point”. This command does not work if you haven’t set your “home point” yet, set it with `/sethome` first. Requires the “home” privilege
- `/killme`—Kills yourself

## See also

- [For Server Hosts](/for-server-hosts)
