---
title: Linux
---

# Linux Configuration

## Obtaining a server binary

{{< notice info >}}
If you are going to be running a Luanti server on a non-headless system, using the regular `luanti` binary with the `--server` argument works the same as `luantiserver`. If you are on a headless Linux system without a graphical environment, `luantiserver` is recommended as it is smaller and does not have extra dependencies that are unnecessary for a server.
{{< /notice >}}

Usually your Linux distribution of choice will have `luantiserver` in its official repositories. However this package may be several releases outdated, so obtaining a server binary through other means is to be recommended.

- **Pre-built third-party server binaries**: [The luantiserver repository by ROllerozxa](https://github.com/rollerozxa/luantiserver) contains up-to-date prebuilt x86_64 headless server binaries that should work on any relatively recent glibc-based Linux distro, built from an automated CI workflow that you can fork if you want to customize the build options.

- **Building from source**: Build Luanti from source with `-DBUILD_CLIENT=0 -DBUILD_SERVER=1`. For more information see [Compiling a headless Linux server](/for-engine-devs/compiling/linux-server/).

- **Docker**: Follow [this guide](/for-server-hosts/setup/docker/).

## Running the Server

{{< notice warning >}}
As of 5.8.0, Luanti no longer ships with a default game, meaning you will have to provide your own game to use with the server. You can browse the games on [ContentDB](https://content.luanti.org/packages/?type=game) and download one to put in the `games/` folder for your server.
{{< /notice >}}

1. Open a terminal.
2. Navigate to wherever you've put your Luanti server files (referred to as /Luanti/ from now on), and run the server with `/bin/luantiserver`.
   - If you want to specify a specific game ID, the game ID choices are located in `/Luanti/games/`. Add in `--gameid yourGameId`\*\* to the end of the command.
   - If you get the error "Multiple worlds are available.", the world choices are located in `/Luanti/worlds/`. Add in `--worldname yourWorldName`\*\* to the end of the command.
3. If your server crashes, then look in the `debug.txt` log file in `/Luanti/`
4. Make sure you make your server [safe from damage](#Protecting_your_server).

For easier server management (and also automatically restarting the server when it crashes) you can create a file named run_server.sh or the like in the /Luanti/ folder with this:

```bash
#!/bin/bash

while true
do
    ./bin/luantiserver --gameid <game> --worldname world --terminal
    sleep 2
done
```

If you're on a headless server you'd want to run the server in some kind of multiplexer like `screen`. Run `screen -h 10000 -S luanti ./run_server.sh`, `Ctrl+A Ctrl+D` to detach, and `screen -r` to reattach at a later date.

If you're going to be running multiple servers you may want to separate their files between each other. One good way to do that would be to make a servers/ folder as a sibling to your bin/ folder, make a folder for each server, and use something like this for running the server:

```bash
#!/bin/bash

while true
do
    ../../bin/luantiserver --world "world" --config "server.conf" --logfile "server.log" --terminal
    sleep 2
done
```

It will store the server's world, config and log contained within its own folder such that it won't conflict with another server.

## Firewall

You most likely know if you have a firewall installed and configured (e.g. iptables or ufw). Be sure to allow the Luanti server to communicate over UDP on the specified port.

## Next Steps

Continue on to [next steps](/for-server-hosts/setup/#port-forwarding).
