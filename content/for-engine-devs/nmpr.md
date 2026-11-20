---
title: Luanti NMPR
aliases:
  - /Engine/NMPR
  - /engine/nmpr
---

The Luanti engine is built on a small core, that was the original network multiplayer release of luanti (let's call it NMPR; the 2010-10-24 version). As the current code still largely bases on the NMPR, it is useful to look at how it works for getting a basic understanding of the engine.

Being around 10000 lines of code, it contains:

- The **Map**: Voxel storage + lighting + rendering
- The **Environment**: Contains the map and the players, handles the simulation of the world
- The **Client** + **Server** logic
- The **main loop**: Invokes the client, the server, the environment and the rendering.
- A bunch of wrappers for OS-dependent things, and utilities.

NMPR has been made available at https://github.com/celeron55/minetest_nmpr which is the easiest to build.

The older source release for the corresponding revision is [available here](https://archive.org/download/minetest_old_sources_2010_archive/minetest_10-10-24_16%3A33%3A41_wonderful.zip) (apply [this patch for Linux](http://gist.github.com/4578183)), and the original win32 build [is available here](https://archive.org/download/minetest_old_releases_2010_through_2012_mostly_win32/2010-10-10_to_25/minetest-c55-win32-101024164856.zip) (works in Wine).

## Map (the voxels)

![Luanti Voxel Storage](/images/minetest_voxel_storage.webp)

- The main content of the Map is a `map<v2s16, MapSector*>` container.
- The main content of a MapSector is `map<s16, MapBlock*>`.
- The main content of a MapBlock is a linear array of 16x16x16 MapNodes.

These form a relatively performant storage of voxel data. In addition to these containers, the latest fetched MapBlock is cached in each MapSector, and the last fetched MapSector is cached in Map, resulting in very useful sequential access speed through the whole abstraction layer.

## Environment

The content of the environment in NMPR is:

- Map
- List of Players

Players contain:

- Position and speed
- An Irrlicht scene node (that is rendered by Irrlicht)
- move() method with collision detection

In later versions of Luanti, the environment also contains things like ActiveObjects and ABMs.

## Network protocol

The high-level network protocol of NMPR is delightfully simple. There are four commands for the server, and four commands for the client. Since this, a lot has been added and changed, but the basic idea stays the same.

### Client to Server

| Name                | Arguments                      | Description                                           |
| ------------------- | ------------------------------ | ----------------------------------------------------- |
| TOSERVER_GETBLOCK   | `v3s16 p`                      | Ask the server to send the data of a block            |
| TOSERVER_ADDNODE    | `v3s16 p, MapNode node`        | Inform the server of a placed node                    |
| TOSERVER_REMOVENODE | `v3s16 p, MapNode node`        | Inform the server of a removed node                   |
| TOSERVER_PLAYERPOS  | `v3s32 p*100, v3s32 speed*100` | Inform the server of the position of the local player |

### Server to Client

| Name                | Arguments                                                      | Description                                  |
| ------------------- | -------------------------------------------------------------- | -------------------------------------------- |
| TOCLIENT_BLOCKDATA  | `v3s16 p, MapBlock data`                                       | Send the content of a block (16x16x16 nodes) |
| TOCLIENT_ADDNODE    | `v3s16 p, MapNode node`                                        | Add a node                                   |
| TOCLIENT_REMOVENODE | `v3s16 p, MapNode node`                                        | Remove a node                                |
| TOCLIENT_PLAYERPOS  | `foreach(player){u16 player_id, v3s32 p*100, v3s32 speed*100}` | Update players on client                     |

luanti uses it's own reliability layer on top of UDP. It isn't well documented at the moment, and thorough understanding of it isn't that important, so let's skip it as of now.

## Server

The NMPR server logic is 287 lines of code that runs in two threads:

**Main thread:**

- Run a simulation step of the environment (= move players)
- Store the time passed from last time for the server thread

**Server thread**

- Receive and handle network packets
- Handle network timeouts
- Send player positions

#### Packet handler

The packet handler handles the TOSERVER\_\* commands coming from clients.

| Name                | Description                                                          |
| ------------------- | -------------------------------------------------------------------- |
| TOSERVER_GETBLOCK   | Serialize the content of a MapBlock and send it (TOCLIENT_BLOCKDATA) |
| TOSERVER_REMOVENODE | Set a node to be air and echo to other clients                       |
| TOSERVER_ADDNODE    | Set a node to the type provided and echo to other clients            |
| TOSERVER_PLAYERPOS  | Update the position and speed of a player in the server environment  |

## Client

Most of what the client does is very obvious, but there is one thing to note:

- **NMPR**: The client "catches" accesses to unknown MapBlocks, and requests them from the server based on those accesses
- **Later**: The server sends MapBlocks based on where the client's player is located and what it hasn't sent yet.

### Rendering

Rendering of players and GUI is done by regular Irrlicht stuff ([Irrlicht tutorials](http://irrlicht.sourceforge.net/tutorials/)). What is interesting is how the voxel world is rendered.

In NMPR, rendering of the Map works like this:

**Cache step** (runs in the background, in a thread managed by Map)

- List MapBlocks in displayed area that have been modified
- Update lighting in them
- foreach(MapBlocks that were modified in the previous steps):
  - Go through the MapBlock, detecting all air/non-air surfaces, collecting them to a list of faces

**Render step**

- Create a list of MapBlocks in rendering range
- foreach(MapBlocks in rendering range):
  - If it is not in front of the player, skip it
  - Draw the faces of the MapBlock

This is how Luanti works up to this day, except:

- At some point, instead of storing faces, Luanti was made to store [meshes](http://en.wikipedia.org/wiki/Polygon_mesh).
- The mesh generator is managed by the Client.
- In Luanti 0.3.1, [occlusion culling](http://en.wikipedia.org/wiki/Hidden_surface_determination#Occlusion_culling) was added to the render step
- In Luanti 0.4.3, the list of MapBlocks to be rendered is cached, and sorted by texture

### Main loop

**Initialization:**

- Basic game stuff: Initialize graphics, load textures, set up camera, hide cursor
- Start server if hosting a game (or playing a local game)
- Start client and connect it to a server

**Loop**

- Read input
- Run client (also steps the environment)
- Run server
- Update camera
- Calculate what block is the crosshair pointing to
- If the player left/right clicked, send a remove/add node command to server
- Render scene
