---
title: Windows
---

# Windows Configuration

## Obtaining a server binary

The regular Windows builds provided on the [download page](https://www.luanti.org/en/downloads/) work fine, running it as `luanti.exe --server` to access the server portion of it.

## Running the Server

{{< notice warning >}}
As of 5.8.0, Luanti no longer ships with a default game, meaning you will have to provide your own game to use with the server. You can browse the games on [ContentDB](https://content.luanti.org/packages/?type=game) and download one to put in the `games/` folder for your server.
{{< /notice >}}

1.  Open command prompt by going in the Luanti installation folder. Then in the "bin" folder, click the blue "File" icon in the top left of the screen. In the drop down menu click "Open Windows Powershell here".
2.  Type this: `.\luanti.exe --server`.
    - If you get the error "Multiple worlds are available.", use `.\luanti.exe --server --worldname `**`world_name`** instead, where **`world_name`** is the name of the world.
3.  If your server crashes, then look at the `debug.txt` in `/Luanti/bin/`
4.  Make sure you make your server [safe from damage](#Protecting_your_server).

If you don't like to start the crashed server, simply start the server out of a batch file which contains the following code:

```batch
@echo off
:crash
luanti.exe --server --worldname <world_name>
goto crash
```

## Firewall

The Windows Defender firewall will prompt you whether Luanti should be allowed access. This may even pop up the first time you play singleplayer in the client, and if you did not grant it permission you will need to open the Windows Defender Firewall with Advanced Security and accept it from there.

![](/images/Windows_defender_minetestserver.webp)

## Next Steps

Continue on to [next steps](/for-server-hosts/setup/#port-forwarding).