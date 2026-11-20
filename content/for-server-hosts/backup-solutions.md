---
title: Backup Solutions
aliases:
  - /Backup_Solutions
  - /backup-solutions
---

It is always important to backup data you and others might care about. For players, backing up a world is as simple as copying the world folder from `luanti/worlds` to a different folder. The backup can then be loaded by copying that folder back into `luanti/worlds`. The rest of this document covers advanced backup solutions for server hosts.

## World Backups

Worlds will usually be contained in the Luanti user data directory under the `worlds` directory, unless you run a dedicated server and explicitly specify a path to the world. The world is a folder containing various files associated with the world.

With the default SQLite backends it will store e.g. map data in a `map.sqlite` file. If you have changed the database backend to something else such as PostgreSQL then it will be stored separately by the Postgres database server, and generally all management and backup advice for these database engines also applies to their usage in Luanti.

Below is more detailed advice when doing backups with the default SQLite configuration.

### Live backups

If you are running a server then it would be nice to take live backups of the world without needing to shut down the server, so they can be done more regularly without causing players to be kicked. It is however still recommended to make cold backups of the world while the server is shut down to guarantee that you produce consistent backups of the world.

> [!WARNING]
> While SQLite databases are a single file (or a couple files, when WAL is enabled), you _should not_ simply copy them while the server is running as the resulting backup will likely be corrupted if writes are being made in the meantime.

You should use `VACUUM INTO` with the `sqlite3` CLI if you want to create live backups of the database. This command makes a vacuumed copy of a SQLite database into another file, and is a lot safer than simply copying the database file as it is done transactionally and creates a consistent snapshot of the original database [(see the SQLite documentation)](https://www.sqlite.org/lang_vacuum.html#vacuum_with_an_into_clause).

This is an example of a Bash script that can be used to copy the world files of `world` and make backups of the SQLite databases to a new backup directory `.BACKUP` next to the world's directory, which could subsequently be compressed and versioned by timestamp:

{{% comment %}} cspell:disable {{% /comment %}}

```bash
#!/bin/bash

bakdir=".BACKUP"

mkdir -p "${bakdir}/"

for db in world/*.sqlite; do
    base_name=$(basename "$db")
    sqlite3 "${db}" "VACUUM INTO '${bakdir}/${base_name}';"
done

cp world/*.txt world/world.mt "${bakdir}/"
```

{{% comment %}} cspell:enable {{% /comment %}}

If you have mods that write more text files to the world folder rather than using mod storage you would add it to the list of files to copy in the final line.

### Cold backups

If you just want to back up a singleplayer world then it is relatively easy to make a cold backup when Luanti is not running. Various methods from copying the directory in your file manager to creating a compressed archive will work.

## Mod backups

When you enable mods to a world, a list of enabled mods will be written to `world.mt` in the world's folder. Luanti now throws an error if mods that are supposed to be enabled aren't available, so if you have a world with a few amount of mods you need to restore then you can simply take this list of missing mods and install them from the main menu content browser.

For more elaborate mod sets or for servers where you would want to keep track of specific versions of mods to use for a world, then versioning your mods using Git would be useful. You can add repositories inside another Git repository using [Git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) which will also pin it to a particular commit in the upstream repository. You can keep track of upstream activity manually, or use Dependabot for GitHub which can automatically create pull requests (configurable) when new commits in submodules are detected.

## Other user data

Luanti saves some other data to the user folder that may be desired to be backed up.

- `client/serverlist/favoriteservers.json` is a list of favorite servers that show up at the top of the server list in the main menu.
- `client/mod_storage.sqlite` is where mod storage for client-side mods is stored, if it has been enabled.
- `screenshots/` contains any screenshots you have taken.
- `mod_data/` contains per-mod data that is not specific to a particular world.
- `minetest.conf` will contain all the settings you have changed.
