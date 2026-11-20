---
title: Reporting Bugs
aliases:
  - /Reporting_bugs
  - /reporting-bugs
---

### Notes

Luanti is never finished, it is constantly a work in progress. Thus all bug reports trivial, or not, help us making the engine better for everyone.

### Check for similar reports

A bug might already be reported or even fixed by the time you are having problems. Please check the following sources for solution prior reporting:

- [Newest Luanti version](https://www.luanti.org/downloads/) - are you using the most recent one?
- [GitHub issue tracker](https://github.com/luanti-org/luanti/issues?q=is%3Aissue)
- [Libera IRC (English)](https://kiwiirc.com/nextclient/irc.libera.chat:+6697/#luanti) (answers might take a while)
- [Luanti Forums: 'Problems'](https://forum.luanti.org/viewforum.php?f=6)

### Gathering information

If you cannot find any similar reported bug, please collect as much relevant information to the developers. More information usually means faster issue tracking. Usually it helps us a lot to provide following information.

- Luanti version (ex. "0.4.17.1", "5.1.1", "5.1.0-dev-numbers")
  - This is displayed in the window title bar.
  - For advanced users: provide the output of `luanti --version`
- Operating system and version (ex. "Ubuntu 18.04", "Windows 10", "Android 9")
  - Please be as precise as possible.

Following information can be found in the system settings / system information or in the task manager:

- CPU type (ex. "Ryzen 7 3700X", "Core2 Duo E8600")
- Amount of installed RAM
  - You might also want to monitor the RAM usage while reproducing the bug.
- Graphics card (ex. "Intel HD Graphics 4000", "GTX 2080 Ti Super")
  - If you can join a game, please provide the OpenGL version (see title bar).
  - For advanced users: driver version and date.

**Most important:** Luanti generates a `debug.txt` file which is located near to the `bin` folder, or `~/.minetest/` for system-wide installations. Copy the last 50 lines or so of the debug file.

If you are tech-savvy, you might also want to look for similar issues in [Troubleshooting](/for-players/troubleshooting) and try to find out the cause of the issue by clearing all settings, tweaking settings or disabling mods.

### Bug report template

If you would like to submit to the Luanti Forums, please consider using this template while doing so:

```
 Topic title: shortly state what bug you are experiencing

 Topic body: state, and give as many details as possible about the bug you are experiencing.
 Luanti version:
 Operating system:
 Computer specs: CPU, RAM, graphics card

 [spoiler=debug.txt]Paste debug.txt here[/spoiler]

```
