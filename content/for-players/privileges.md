---
title: Privileges
aliases:
  - /Privileges
  - /privileges
---

Every player has a set of privileges, which differ from server to server. Roughly spoken, one’s privileges determine what one is able to do and what not. Each privilege has a name (the meaning is described below). Privileges can be granted and revoked from other players by any player who has the privilege called “privs”. On a multiplayer server with a default configuration, new players start with the privileges called “interact” and “shout”. To view one’s own privileges, one can issue the [server command](/for-players/server-commands) “/privs”.

## Built-in privileges

As of version 5.0.0, Luanti comes with the following privileges:

- gameplay-related:
  - **interact**—can build/mine/use blocks and drop/eat/craft/use items and punch things and interact with objects and players
  - **give**—can use the `/give` and `/giveme` commands
  - **teleport**—can use the `/teleport` command to teleport oneself to certain coordinates or to another player
  - **bring**—in combination with _**teleport**_, can use the `/teleport` command to teleport any player to certain coordinates or to yet another player
  - **fast**—allows the player to activate fast mode
  - **fly**—allows the player to activate fly mode
  - **noclip**—allows the player to activate "noclip" mode, which allows them to fly through walls
- chat-related:
  - **shout**—can [chat](/for-players/chat) with other people
- world–manipulation-related:
  - **settime**—can set time of day using `/time`
- moderation-related:
  - **privs**—can set any privileges of players using `/grant` and `/revoke`
  - **basic_privs**—can set the privileges set as basic_privs in the minetest.conf (default “interact” and “shout”) using `/grant` and `/revoke`
  - **kick**—can kick players with `/kick`
  - **ban**—can ban/unban IPs and names using `/ban` and `/unban`
  - **rollback**—can use the rollback functionality
  - **protection_bypass**—can bypass protection of blocks (e.g. can open locked chests or steel doors of everyone)
- administration-related:
  - **server**—can do server maintenance stuff such as `/shutdown`, `/clearobjects`, `/set`, …
  - **debug**—can access advanced [debug](/for-creators/debug) features and information, such as the wireframe in the debug screens (F5). It also prevents the game from restricting information in the debug screen and from restricting the “toggle block bounds” key.

## Irrevocable privileges

A player’s privileges may be irrevocable in certain situations. It is not possible to revoke these privileges with `/revoke` then.

In multiplayer [servers](/for-players/servers) setting “name” automatically has all privileges and all of these are irrevocable. This is also the case for players who started a server (not a dedicated server). In singleplayer, you start with **interact**, **shout**, **privs** and **basic_privs**. These privileges are irrevocable.

## Privileges from mods and games

### Minetest Game

- **home**—can use `/home` and `/sethome`.

### Mods

[Mods](/for-players/mods) may make additional privileges available on the server. Issue the server command `/help privs` to receive a full list (and short descriptions) of all possible privileges on the server.

## Server configuration

Using the server’s configuration files, a lot of privilege-related stuff can be manipulated.

There is an option in the configuration file for setting the default privileges for new players. `default_privs = interact, shout`

- The player having the name in the “name” field of the configuration has all the privileges.

## See also

- [Servers](/for-players/servers)
- [Server commands](/for-players/server-commands)
- [For Server Hosts](/for-server-hosts)
