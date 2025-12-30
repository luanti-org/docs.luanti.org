---
title: Installing Mods
aliases:
  - /Installing_Mods
  - /Installing_mods
  - /installing-mods
---

# Installing Mods

## Security considerations

**Mods are by default run in a secure environment that restricts access to the filesystem and execution of external programs, however not all sandboxes are 100% secure. Additionally do not mark mods as trusted or disable mod security altogether if you are not aware of what you are doing, as they will run un-sandboxed at the same privileges as Luanti. This applies not only to malicious mods but benign ones with security vulnerabilities. Exercise caution when downloading mods outside of official channels such as ContentDB where mods are vetted by the community for safety and security.**

## Installing a mod

Mods can be installed from ContentDB in the "Browse online content" button in Luanti 5.0+. Mods downloaded this way are automatically installed and get checked for updates. See [Installing content](https://content.luanti.org/help/installing/).

For mods downloaded manually, or on a server, you would clone the source repository or extract the Zip archive into your mods folder.

To find your mods folder, go into the "About" tab and press the "Open User Data Directory" button. If there isn't already a mods folder there then create it.

After extracting the mod there you need to enable it for your world. This can either be done in the GUI by clicking on “Configure” in the world selection, or by adding `load_mod_<modname> = relative/path/to/mod` (example `mods/xcompat`) in the `world.mt` file in the world directory. Previous to version 5.6.0 this was a boolean setting.

Note that newly installed mods are disabled for all worlds by default, so you explicitly need to enable them.

## Additional install directories (all Luanti versions)

Other places to install mods are `world-directory/worldmods/` and `$path_share/mods/`. `$path_share` and `$path_user` are only relevant to system-wide installs of Luanti (currently, possible only on Linux). As mentioned above, Luanti on Windows and portable builds operate within their install directory itself, which corresponds to both `$path_share` and `$path_user`.

Note that users should generally install mods in the normal install directory and not in the additional ones. Note that having copies of the same mod in different places may easily generate mod conflicts.

Differences between the two kinds of places mods can be loaded from:

- In the `/mods` folder technically parallel to the `/bin` folder the executable is in.

On different installations this may very well also be in some other Luanti location such as a shared, user or hidden folder. Only mods in this place are **toggle-able**. Mods in this folder can be run with any world created by any game. This is therefore an easy place to create mod conflicts that might even crash Luanti.

- In a `/worlds/<name_of_some_world>/worldmods` folder inside the sub-folder of a specific world.

Mods in a worldmods folder are **not toggle-able** and will run on and only on that specific world, and cannot be accessed from any other world.
