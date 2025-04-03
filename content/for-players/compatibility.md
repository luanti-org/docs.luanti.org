---
title: Compatibility guarantee (for players)
---

# Compatibility guarantee (for players)

Luanti tries to guarantee backwards compatibility so you can usually
safely upgrade your client and continue playing the games and joining the servers you like.

{{< notice tip >}}
Check out the compatibility notes in [the changelog](https://docs.luanti.org/about/changelog/)
before upgrading.
{{< /notice >}}

## Client and Server

Newer clients should be able to connect to and be able to play without issues on older servers,
"downgrading" largely to the feature set of an older client.
(Some features however can be implemented purely on the server or on the client,
making them automatically compatible with older client and server versions respectively.)

Older clients should be able to connect to and be able to play on newer servers,
so long as they do not use any newer features they do not support.

This means that, so long as the game remains the same,
it is possible to safely upgrade client and server independently.

## Games and Mods

Backwards compatibility applies to the continued functioning of old *games*
(or "mod soups" more broadly) -
sets of mods that were written against an older Luanti version.

This means that there may be new features which are, possibly necessarily,
incompatible with the assumptions older mods may make, if they are used at all.

Hence you may have a working configuration, add a new mod enabling/using one of these new features,
and by this cause issues with existing old mods.

For example Luanti 5.9 allowed omitting punchers.
Mods that were written against older versions may expect there to always be a puncher.
If a new mod now omits the puncher (as it may), this may cause a crash in an older mod,
which is inevitable: The older mod needs to decide what to do in case of a missing puncher.

Backwards compatibility is not absolute.
Sometimes old mods or games will break because a bug has been fixed,
or because a minor breakage has happened following a deprecation.
In these cases, you should try updating your mods and games,
and if the problem persists, report the issue to the respective maintainers.

## Worlds

Newer Luanti versions will be able to open old worlds.
This is taken extremely seriously and goes back more than a decade.

However, once you open a world with a newer Luanti version,
that world need no longer be compatible with older Luanti versions,
e.g. because compression has been upgraded or legacy serialization formats
have been upgraded to more modern ones.

If you want to be on the safe side, make regular backups of your worlds.

## Command-Line Interface

The backwards compatibility guarantee does currently not extend to the command-line interface,
which should not be considered stable (but nevertheless, for the most part, doesn't change very much).

## See also

- [Compatibility guarantee for creators](/for-creators/compatibility)
