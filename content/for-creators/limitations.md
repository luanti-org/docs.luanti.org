---
title: Limitations
aliases:
  - /limitations
---

This page lists various limitations that exist in the Luanti engine. This list could also be seen as a wishlist for far fetched features.

Also see the [List of hardcoded features](/for-creators/list-of-hardcoded-features) page for behavior in the engine that is mostly hardcoded.

## Running code on the client

Mods always run on the server, and not on the client. While there is a client modding API (with client-side mods, CSMs), they are very limited and require to be manually installed. The concept of SSCSM (Server-sent client side modding) would solve this but has not been fully implemented yet.

## Checking arbitrary keys

There is no way to check arbitrary keys in the scripting API. You are limited to the existing keybinds that are available in `get_player_control`. Aux1 is a generic keybind that you could give a custom use for.

## Changing node textures on the fly

While there are ways to e.g. colorize nodes using param2, you cannot change the texture of a node itself. You will need to register a separate node with different textures, and swap between these nodes.
