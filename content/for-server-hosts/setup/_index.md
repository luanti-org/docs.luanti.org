---
title: Setting up a server
bookCollapseSection: true
aliases:
  - /Setting_up_a_server
  - /Setting_up_a_server/Debian
  - /setting-up-a-server
  - /server/setup
---

# Setting up a server

This page is a guide, separated into sections, for setting up a Luanti server. It assumes you want to run a server that is publicly facing the Internet, as compared to a LAN server if you want to play with players on the same network.

## Choosing Hardware

It's recommended that you use a VPS or dedicated server to host a game server which you want to make publicly available. Residential Internet connections tend to be unreliable and also have less upload speed. You may also not be able to keep a server online 24/7 when hosting from home.

That being said, hosting from home will work fine if you have some hardware you can keep online, and you have a good enough Internet connection. Keep in mind that if you are behind [CGNAT](https://en.wikipedia.org/wiki/Carrier-grade_NAT), you are unable to host a public server from home at all as it won't be accessible to the wider internet.

Luanti does have a _Host Server_ option in the main menu, which is fine for temporary servers for a couple of friends, but if you want to run a public server you would want to run it dedicated as it can be kept online independent of the client being active.

## Your IP Address

If you already know what your IP address is, proceed onwards, otherwise [this guide](/for-server-hosts/setup/ip-address/) should assist you in determining what your IP Address is.

## Installation, Configuration, and Setup

Please see the respective guide for your platform:

- [Linux](/for-server-hosts/setup/linux/)
- [Windows](/for-server-hosts/setup/windows/)
- [TrueNas](/for-server-hosts/setup/truenas/)

### Port forwarding

If you are self-hosting a server you will usually need to port forward the server in your router for it to be accessible to the outside internet. This isn't necessary for LAN play, as you'll simply provide the internal IP of the server accessible within the local network.

1.  Choose a port to run the server on. The default of 30000 is recommended, and if you host several it is recommended to increment upwards. (e.g. 30001, 30002...)
2.  Login to your router, and port forward UDP on the chosen port for your server computer. Depending on your router the port forwarding page may show you a list of devices connected with their hostname visible, otherwise you will need to input the internal IP of the computer you are hosting the server on.
3.  Alter any computer firewalls you may have to allow traffic to the port you choose.

### Server list

Make your server listed in the server list and by setting the following settings in minetest.conf:

- `server_announce = true` - makes Luanti announce the server to the server list.
- `server_name` - set the value of this to your server's name.
- `server_description` - set the value of this to a longer description describing your server.
- `server_address` - if you have a domain name for your server, then set this to the domain name.
  - Should _only_ be the domain name (e.g. `coolserver.luanti.org`, not `https://coolserver.luanti.org/woah/`). If you do not have a domain name pointed to your server then do not add this setting or write anything for it, or the server list will reject your server.
- `server_url` - if you have a website for your server, then set this to the website URL, it must begin with `http://` or `https://`.
- `motd` - a message that is sent to the player when they join. Use this to welcome them.
- `serverlist_url` - do _not_ set this unless you are announcing to a custom list.
- `bind_address` - do _not_ set this unless you have a setup with multiple IP addresses on the same server.
- `ipv6_server` - set this if you have domain name and an IPv6 address.
- _Note:_ You need to restart to make changes in minetest.conf take effect.

### Remote Media

While not required to set up a server, this is highly recommend for servers with lots of media and/or large media assets. See [this guide](/for-server-hosts/remote-media/) for more details.

## Protecting your server

### Protecting the Luanti world/server

When setting up a new server, you should consider which protections are needed. **This is extremely important for public servers**, because you cannot predict who will connect or what they will do on your server.

Common problems include:

- Accidental or intentional damage to other players' work (griefing).
- Chat spam (may include swearing or advertisements).
- Aggression, harassment or other unwanted behaviors between players.
- Trouble makers who evade bans.
- Impersonation of well known people within the Luanti community.
- Bugs, for example caused by mods, which allow a malicious player to execute arbitrary system commands.

Many of these problems can be removed or minimised by advanced planning and awareness:

- Install a protection mod, such as [areas](https://content.luanti.org/packages/ShadowNinja/areas/) or [protectors](https://content.luanti.org/packages/TenPlus1/protector/). These allow players to protect areas, which cannot be changed by other players.
- Enable rollback by adding `enable_rollback_recording = true` to minetest.conf. Rollback can tell you which player placed a node, and allows a player's actions to be reverted.

{{< notice warning >}}
The engine rollback functionality is very limited and can't roll back griefing caused by mod-made node changes (e.g. spawning a bunch of trees or covering things in water). In addition to rollback, you should also always make regular backups of the map database.
{{< /notice >}}

- Install a mod to help you manage bans, such as [xban2](https://content.luanti.org/packages/kaeza/xban2/).
- On the other hand, if you are setting up a private server, install a whitelist mod such as [whitelist](https://content.luanti.org/packages/Zughy/whitelist/).
- Create rules for your server and make sure you have enough time (or a team of moderators) to supervise your server and watch for players who breaks your rules.
- Never grant privileges to a player just because they use a name you recognise. Player names are not reserved between servers, so you should always confirm who the player is.
- Install and configure a chat filter mod like [filter](https://content.luanti.org/packages/sofar/filter/), to prevent swearing and/or sexual chat. This is useful to inform players of the rules rather than actual enforcement, as working around filters is fairly easy.

For more mods useful to server administration, see the [Server Moderation and Tools](https://content.luanti.org/packages/?tag=server_tools) tag on ContentDB. See also the [SzUtilPack](https://content.luanti.org/packages/Warr1024/szutilpack/) package which is a modpack by Warr1024 that contains a lot of useful mods for server management such as automatic restarts, pruning players or additional useful chat commands.

### Protecting the host machine

- Standard advice on Linux/Windows server security applies.
- On Linux you can add a dedicated user for running the Luanti server.
- Never, ever, disable mod security.
- Make sure you trust mods you add to secure.trusted_mods

## Managing your server

### Server Configuration

For a detailed explanation of the server configuration file, see [minetest.conf](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example).

You may also want to consider to use a different [database backend](/for-server-hosts/database-backends) for your world.

### Server Commands

Running /help in-game will show you a list of the built-in engine commands as well as any server commands installed by mods.

### Privilege System

See the [privileges](/for-players/privileges) page for detailed information on the privilege system.

## See also

- If your server has lots of media it may be useful to set up a [remote media server](/for-server-hosts/remote-media) for it.
- The "[Illustrated Server Creation Guide](https://forum.luanti.org/viewtopic.php?f=10&t=2870)" forum thread contains interesting discussion.
- See this forum thread for [Server performance settings](https://forum.luanti.org/viewtopic.php?f=10&t=1825)
