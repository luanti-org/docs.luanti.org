---
title: Objects
aliases:
  - /Engine/Objects
  - /Unknown_Object
  - /engine/objects
  - /objects
---

# Engine/Objects

In Luanti, an object is a moving thing in the world. The two types of objects are players and entities.

A static object is one that is saved in a mapblock, and an active object is one that is currently loaded and updating in the environment.

## ActiveObjects

ActiveObjects aka objects are objects in the environment, consisting of roughly everything that is not part of the voxel world (=Map).

`SAO = ServerActiveObject CAO = ClientActiveObject`

`serverobject.{h,cpp} clientobject.{h,cpp} content_sao.{h,cpp} content_cao.{h,cpp}`

All objects have a server- and a client-side counterpart. The server-side object types that currently exist are

- TestSAO (a small test thing)
- ItemSAO (old item; not used)
- LuaEntitySAO (all entities defined in Lua; basically everything other than the players), and
- PlayerSAO (the player objects which enable standard object interaction for players. Players also have a ServerRemotePlayer or something like that which handles the things in which players are different to other objects).

Client-side objects:

- TestCAO (obvious)
- ItemCAO (obvious)
- GenericCAO (everything else; LuaEntitySAOs and PlayerSAOs register as this on the client-side. This enables them to have the exact same capabilities in terms of looks, client-side interaction and stuff.)

If eg. implementing meshes to go through the regular Lua->server->client pipeline, one'll want to implement them in GenericCAO, through ObjectProperties. ObjectProperties should contain the filename of the mesh (which should be transferred like images), and possibly some small additional information.

All the players on the server have an associated PlayerSAO object, of which generally all will appear on clients as GenericCAO client-side objects.

The CAO of each player also exists on the player's clients itself, just like the other players. It is always hidden and is used for nothing. It doesn't cause any considerable overhead and filtering it out server-side would make the systems messier. On a client, the local player is a separate thing (a LocalPlayer, added to the environment in Client::Client(); it is similar to the ServerRemotePlayers mentioned earlier).

## Unknown Object

![](/images/Unknown_Object.png)

Texture of an unknown object.

An **unknown object** is pseudo-object in Luanti to represent an object (such as a [mob](/for-players/mobs)) of which the object definition is unknown. These objects should never appear in the game, and it's always an error when you encounter one.

## Behavior

An unknown object appears as a flat texture with “unknown object” written on it. The velocity of the original object is usually preserved so unknown objects often tend to fly through the world.

Internally, an unknown object still knows the “real” object it represents and the associated data. You can see its entity/object ID by [pointing](/for-players/pointing) it. If an unknown object is found by Luanti, there will also be an error message complaining that a LuaEntity could not be found (this “LuaEntity” refers to the unknown object), along with its entity ID.

To fix problems with unknown objects, check the troubleshooting section below. Unknown objects are destroyed when punching or if they receive any amount of damage.

## Troubleshooting

A common reason for an unknown object to appear is when you have previously activated a [mod](/for-players/mods) which added some new objects, then later deactivated said mod. Now all objects from this mod will appear as an unknown objects. In this case, you can solve this simply by enabling the missing mod again. If you forgot from which mod this object originated from, point it to learn its entity ID. The part before the column is the mod name.

Another possible cause is a bug in mods or games. Developers of a game may have made a mistake or they removed object types intentionally without any replacement. Complain to the game authors if this happens, as this is generally considered poor development practice. If unknown objects occur without you using any mods, this is almost certainly a bug.

## See also

- [Unknown Item](/for-players/items#unknown-item)
- [Unknown Node](/for-players/nodes#unknown-node)
