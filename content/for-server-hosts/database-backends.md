---
title: Database backends
aliases:
  - /Database_backends
  - /database-backends
  - /server/database-backends
---

# Database backends

Luanti uses a database system for certain world data where the backend that stores the data can be switched out depending on the use cases needed. This page lists all databases and the backends that can be used with them.

## Databases

There are four databases that store data for each world:

- Map
- Players
- Authentication
- Mod Storage

### Map database

The map database stores mapblock data. The backend is set using the `backend` setting in `world.mt`.

To migrate it to a different backend, the `--migrate` argument is used.

### Player database

The player database stores information about in-world data for players. The backend is set using the `player_backend` setting in `world.mt`.

To migrate it to a different backend, the `--migrate-players` argument is used.

### Authentication database

The authentication database stores player credentials as well as privileges. The backend is set using the `auth_backend` setting in `world.mt`.

To migrate it to a different backend, the `--migrate-auth` argument is used.

### Mod storage database

The mod storage database stores per-world data that is available to mods as a key-value data store. The backend is set using the `mod_storage_backend` setting in `world.mt`.

To migrate it to a different backend, the `--migrate-mod-storage` argument is used.

## Migrating backends

The Luanti server is able to migrate backends when running from the command-line, meaning either `luantiserver` for headless server builds or `luanti --server` for client builds, and `luanti.exe --server` for Windows.

To migrate a database to a new backend, use the corresponding argument mentioned above for the database, the backend name you want to migrate to (should be lowercase), and specify the world either using `--world` or `--worldname`. For an example of migrating the players database of a world to a new backend:

```bash
luantiserver --migrate-players <name of new backend> --world <path to your world directory>
```

> [!NOTE]
> Additional configuration in `world.mt` is required beforehand for migrating to the Redis and PostgreSQL backend. See corresponding backend section below for more information.

## Backend comparison table

This table is a rough estimate of what you may expect from each database backend in terms of speed, reliability and compatibility. It is elaborated on in each section, and your mileage may vary from what is listed here.

| Backend    | Speed           | Reliability   | Compatibility with builds\*\*                   | Map | Players | Authentication | Mod Storage |
| ---------- | --------------- | ------------- | ----------------------------------------------- | --- | ------- | -------------- | ----------- |
| SQLite3    | **Good**        | **Good**      | **Always supported**                            | ✔  | ✔      | ✔             | ✔ (5.5.0+) |
| LevelDB    | **Good**        | **Bad**       | **Mostly supported**                            | ✔  | ✔      | ✔             |             |
| Redis      | **Very good**   | **Good**      | **Inconvenient** _(Redis server required)_      | ✔  |         |                |             |
| PostgreSQL | **Very good**\* | **Very good** | **Inconvenient** _(PostgreSQL server required)_ | ✔  | ✔      | ✔             | ✔ (5.7.0+) |
| Dummy      | **N/A**         | **N/A**       | **Always supported**                            | ✔  | ✔      |                |             |
| Files      | **Bad**         | **Bad**       | **Deprecated** _(support may be removed)_       |     | ✔      | ✔             | ✔          |

\* [The PostgreSQL mod storage backend is very slow](https://niklp.net/posts/minetest-postgresql-benchmark/), and its implementation is only intended for those who want to store everything in PostgreSQL nonetheless.

\*\* The _Compatibility with builds_ column is only relevant if you want to distribute the worlds.

## More information about specific backends

### SQLite3

SQLite3 is the default backend for all Luanti backends, it is always supported by engine builds and is therefore also the standard database format that is used to distribute worlds. It is an embedded database that works out of the box without configuration or a separate database server, making it perfect for singleplayer worlds and small servers.

[SQLite as a database has very high reliability](https://www.sqlite.org/hirely.html) and decent performance out of the box, but the performance of the SQLite databases can be further tuned beyond what Luanti does by default. See [SQLite performance tuning](https://phiresky.github.io/blog/2020/sqlite-performance-tuning/) for some advice on how to do so.

### LevelDB

> [!WARNING]
> LevelDB as a database suffers from **serious reliability issues** that are well known both inside and outside of Luanti (See [References](#references) for LevelDB horror stories). Random corruption may occur including to but not limited to during power loss or any other unexpected shutdown where the database is not safely closed. **It should be used with caution.**

LevelDB is an embedded key-value data store that can offer better speeds compared to SQLite. The persistent database is also compressed with Snappy compression leading to smaller file sizes.

LevelDB support is enabled in the official Windows builds and most Linux builds.

### Redis

Redis is an in-memory key-value datastore that can be used both as a cache and for persistent data storage. Luanti needs to be compiled with support for it and uses the `libhiredis` client library to communicate with the Redis server. To set up a Redis server see [Installing Redis](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/).

As of March 2024 Redis switched to a non-free license for future versions. Forks of the final free Redis version, such as [Redict](https://redict.io) and [Valkey](https://valkey.io), may serve as a compatible drop-in replacement for Luanti's Redis support.

> [!NOTE]
> The use of Redis was deprecated in 5.9.0 and was set to be removed in 5.10, but is no longer deprecated (see [Luanti issue #14822](https://github.com/luanti-org/luanti/issues/14822)).

#### Extra world.mt settings for Redis

- `redis_address`: The IP or hostname of the redis server.
- `redis_port`: The port of the redis server. (Optional, default is 6379)
- `redis_hash`: Hash where the MapBlocks are stored in, if you want multiple worlds in one redis instance this needs to be different for each world. If you don't need that you can use e.g. `IGNORED`.

### PostgreSQL

PostgreSQL is an excellent alternative to SQLite for larger Luanti servers. It runs as a separate database server from the Luanti server, can more efficiently manage very large map databases, live database backups are easier to make with [`pg_dump`](https://www.postgresql.org/docs/current/app-pgdump.html), and data can be accessed by other programs to e.g. interface a website in front of your database for server players to use.

It is recommended to use PostgreSQL 9.5+ to provide UPSERT functionality. To improve PostgreSQL's performance with Luanti servers please also configure `postgresql.conf` with a minimum `shared_buffer` of 512MB, with a maximum of 50% of your server's available RAM.

#### Extra world.mt settings for PostgreSQL

- `pgsql_connection`: PostgreSQL connection string for the map database.
- `pgsql_player_connection`: PostgreSQL connection string for the player database.
- `pgsql_auth_connection`: PostgreSQL connection string for the auth database.
- `pgsql_mod_storage_connection`: PostgreSQL connection string for mod storage database.

The connection strings all follow the same format for providing database credentials:

```txt
host=<db_host> user=<db_user> password=<db_password> dbname=<db_name>
```

### MariaDB (WIP)

A WIP MariaDB database backend was implemented in [#13266](https://github.com/luanti-org/luanti/pull/13266), but was abandoned before it could be merged. The addition of a MariaDB backend is approved by the core developers and the pull request may be adopted by someone who is interested in it being available in upstream.

MariaDB allows about the same capabilities as PostgreSQL and has the same qualities as it, running as a separate database server.

To use the MariaDB database backend, the MariaDB C++ connector is required which usually is not part of the MariaDB package. You can find the [source code for it here](https://github.com/mariadb-corporation/mariadb-connector-cpp).

### Dummy

A dummy database backend, which stores all data in RAM without saving it to disk. This means that as soon the server shuts down, all data will be lost. And when you re-join, things such as the map will be re-generated from scratch. However all data will stay persistent in memory while the server is running, compared to a black hole database.

This backend is useful for games and servers which don't need a persistent map, such as Capture the Flag. It can also be useful for development when testing map generation capabilities. Games can configure whether new worlds should be created with the dummy backend for the map database using the `map_persistent` value in `game.conf`.

### Files (Deprecated)

A legacy backend for player and authentication data storing it in a flat file database. It's slow, unreliable and clunky. Use of this backend is discouraged now and it's possible this backend will be removed in the future.

For mod storage, Files uses the legacy JSON-based file storage method as opposed to a regular database system. It is slower, will keep the entire mod storage data in memory and does not support storing binary data.

Luanti will throw warnings when a server is run with the Files backend for a database, and it is recommended to migrate any worlds that use it to SQLite or another database backend.

## References

**LevelDB reliability issues**:

- [Avoiding LevelDB in Minetest for now](https://lazyjminetest.blogspot.com/2014/09/avoiding-leveldb-in-minetest-for-now.html)
- [Issues · google/leveldb · GitHub](https://github.com/google/leveldb/issues?utf8=%E2%9C%93&q=corrupt+is%3Aissue)
- [Unrecoverable corruption in Chromium](https://bugs.chromium.org/p/chromium/issues/detail?id=261623)
- [Corruption in syncthing](https://forum.syncthing.net/t/panic-leveldb-table-corruption-on-data-block/2526/23)
- [Corruption after power loss](https://github.com/google/leveldb/issues/333)
- [Corruption in Ethereum](http://ethereum.stackexchange.com/questions/1159/corruption-on-data-block-while-synchronising)
- [LevelDB reliability?](https://bitcointalk.org/index.php?topic=1394020.0)
- [Does LevelDB still corrupt data?](https://stackoverflow.com/questions/40675204/does-leveldb-still-corrupt-data)
