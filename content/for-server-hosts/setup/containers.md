---
title: Docker / Container
aliases:
  - /for-server-hosts/setup/docker/
---

# Docker / Container Configuration

## Obtaining a server binary

- The official image can be found [here](https://github.com/luanti-org/luanti/pkgs/container/luanti)
  - ghcr.io/luanti-org/luanti:latest - latest stable release
  - ghcr.io/luanti-org/luanti:\<tag> - a specific version (git tag)
  - ghcr.io/luanti-org/luanti:master - latest dev version

- [Warr1024's Docker image](https://hub.docker.com/r/warr1024/minetestserver).

## Running the Server

### Docker

It is recommended that you use [docker compose](https://docs.docker.com/compose) to set up, configure, and run your server

A simple `compose.yml` example:

```yml
services:
  luanti_server:
    image: ghcr.io/luanti-org/luanti:latest
    restart: unless-stopped
    volumes:
      - ./config:/etc/minetest:ro
      - ./data:/var/lib/minetest
      - ./games:/var/lib/minetest/.minetest/games:ro
      - ./mods:/var/lib/minetest/.minetest/mods:ro
    ports:
      - "30000:30000/udp"
```

Before starting the container:

- create the necessary directories:

  ```bash
  mkdir -p data/.minetest config games mods
  ```

- change ownership of `data` to `minetest` user (UID 30000)

  ```bash
  sudo chown -R 30000:30000 data/
  ```

- copy content of the [minetest.conf.example](https://github.com/luanti-org/luanti/blob/master/minetest.conf.example) file to `config/minetest.conf`

- install the game you want to play in `./games/`. Follow [this guide](https://content.luanti.org/help/installing/#installing-using-the-command-line). For example, to install `voxelibre` using `git`

  ```bash
  git clone https://git.minetest.land/VoxeLibre/VoxeLibre ./games/voxelibre/
  ```

- start the container
  ```bash
  docker compose up -d
  ```

`./data/.minetest/` has the directory `worlds/`, where the world will be saved.

`./config/minetest.conf` has settings of the server.

If you have multiple games or worlds, you can specify which one to run in `compose.yml`. For example:

```yml
services:
  luanti_server:
    image: ghcr.io/luanti-org/luanti:latest
    restart: unless-stopped
    volumes:
      - ./config:/etc/minetest:ro
      - ./data:/var/lib/minetest
      - ./games:/var/lib/minetest/.minetest/games:ro
      - ./mods:/var/lib/minetest/.minetest/mods:ro
    ports:
      - "30000:30000/udp"
    command: --config /etc/minetest/minetest.conf --gameid voxelibre --worldname world
```

`--config /etc/minetest/minetest.conf` tells the server where to find the config file inside the container. Keep this flag as-is when using the volume mount from this guide.

To see available flags run:

```bash
docker exec -it luanti_server luantiserver --help
```

### Podman

It is recommend that you use [podman quadlets](https://www.redhat.com/en/blog/quadlet-podman) to set up, configure, and run your server

## Next Steps

Continue on to [next steps](/for-server-hosts/setup/#port-forwarding).
