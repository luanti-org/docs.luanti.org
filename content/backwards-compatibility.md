title: Backwards compatibility guarantee
---

# Backwards compatibility guarantee

Luanti tries to guarantee backwards compatibility, but there are limits to this.

## Client and Server

Newer clients should be able to connect to and be able to play without issues on older servers,
"downgrading" to the feature set of an older client.

Older clients should be able to connect to and be able to play on newer servers,
so long as they do not use any newer features they do not support.
It is on the game developer to decide whether this is game-breaking and older clients have to be prevented from playing,
or whether they can implement sensible fallbacks.
Some features however can be implemented purely on the server or on the client,
making them automatically compatible with older client and server versions respectively.
<!-- observers -->

<!-- what luanti does when you try to use new features with old clients varies -->

This means that, so long as the game remains the same,
it is possible to safely upgrade client and server independently.

## Games and Mods

Backwards compatibility ultimately only applies to the continued functioning of old games,
that is, sets of mods that were written against an older Luanti version.

This means that there may be features which are, possibly necessarily,
incompatible with the assumptions older mods may make.

Hence you may have a working configuration, add a new mod enabling one of these new features,
and by this cause issues with existing old mods.

For example Luanti 5.9 allowed omitting punchers.
Mods that were written against 5.8 and older may index the puncher.
If a new mod now omits the puncher (as it may), this may cause a crash in an older mod,
which is inevitable: The older mod needs to decide what to do in case of a missing puncher.

## Lua API

It should go without saying, but: "Namespaces" and "classes" can and will be extended
by adding new functions or methods.

Maybe there is no `core.frobnicate` today, but there might be tomorrow.
This is why you should use your own namespaces and wrappers
and not try to "extend" engine namespaces or classes yourself.

Similarly, functions that return tables may return tables with *more* fields in the future.
Hence you **should not** iterate over fields:
Just get the fields you want and ignore extraneous fields.

Functions that take tables might support more fields in the future.
Hence you **should not** provide extraneous fields.

Functions that take multiple parameters might take more parameters in the future.
If you're hooking a function,
you should use a vararg to perfectly forward the parameter list, like this:

```lua
local frobnicate = core.frobnicate
function core.frobnicate(...)
	print("frobnication is starting! arguments are:", ...)
	return frobnicate(...)
end
```

Functions that return multiple values might return more values in the future.
This means that when running a post-hook,
you should store all return values in a table ideally:

```lua
local function pack(...)
	return {n = select("#", ...), ...}
end

local frobnicate = core.frobnicate
function core.frobnicate(...)
	local results = pack(frobnicate(...))
	print("frobnication is done! results are:", unpack(results, 1, results.n))
	return unpack(results, 1, results.n)
end
```

Functions that take values of certain types today may take values of different types tomorrow.

Generally, you can not assume that undocumented (but exposed) Lua APIs or behaviors are subject
to the backwards compatibility guarantee.
If you rely on an undocumented feature, please bring it to our attention
by filing a feature request, asking for it to be documented.

## Command-Line Interface

The backwards compatibility guarantee does currently not extend to the command-line interface,
which should not be considered stable.

## Regressions

There will unfortunately always be bugs that accidentally cause backwards compatibility to be broken.
These are called *regressions*: Something that used to work no longer does.

Regressions are not strictly limited to documented behavior:
The documentation is not a practically complete specification,
so exceptions will be made as is sensible.

## Minor Breakages

Luanti will occasionally have minor compatibility breakages as is deemed sensible.
These will usually follow after a deprecation warning has been in for
a couple releases.

If you're a mod developer, please check and act upon your warnings.

## Major Breakages

Planned major breakages are documented in `doc/breakages.md`.
They are to be done in the 6.0 release of Luanti.

## Conclusion

If you are unsure whether something is covered by the backwards compatibility guarantee, ask.
