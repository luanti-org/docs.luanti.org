---
title: Compatibility guarantee (for creators)
---

# Compatibility guarantee (for creators)

Luanti tries to guarantee backwards compatibility to a reasonable extent
so that content usually continues to work as Luanti evolves.

## Deprecations

Deprecations are found along with the documentation of the deprecated features
in the various files in the `doc` folder, in particular,
[`lua_api.md`](https://github.com/luanti-org/luanti/blob/master/doc/lua_api.md).

For many features, there are *deprecation warnings*,
some of which give you a stack trace that tells you where deprecated API usage originated.

{{< notice tip >}}
- For you to see warnings at all in-game, you might want to set `chat_log_level` to `warning` or higher.
- Your `deprecated_lua_api_handling` setting should be set to at least `log`;
  you should even consider `error` if you have a relatively clean game.
- Full stack traces for deprecation warnings are logged at the lower `info` log level,
  so you will want to set `debug_log_level` to `info` to see them in `debug.txt`.
{{< /notice >}}

### Minor Breakages

Despite trying to abide by [semantic versioning](https://semver.org),
Luanti will occasionally have minor compatibility breakages in minor releases (but not in patch releases) as is deemed sensible.
These will usually follow after a deprecation warning has been in for a couple releases.

{{< notice tip >}}
Make sure to follow [the changelog](https://docs.luanti.org/about/changelog/) for compatibility notes
including deprecations and minor breakages.
Test your content on newer engine versions and fix deprecation warnings.
{{< /notice >}}

### Major Breakages

Planned major breakages are documented in [`breakages.md`](https://github.com/luanti-org/luanti/blob/master/doc/breakages.md).
They are to be done eventually in the 6.0 release of Luanti, which is still far off.

## Undocumented behavior

Generally, you can not assume that undocumented (but exposed or observable)
APIs or behaviors are subject to the backwards compatibility guarantee.
You should always try to write your mods such that you only rely on documented behavior.
In particular, try to avoid "cargo culting": When taking code from somewhere else,
make sure to understand *why* it works and how (that) it uses the engine correctly.

If you find that you rely on an undocumented feature,
please bring it to the attention of the Luanti community by
[filing a feature request](https://github.com/luanti-org/luanti/issues/new?labels=Feature+request&template=feature_request.yaml),
asking for it to be documented.
Otherwise it is possible that behavior is accidentally or deliberately changed,
simply because there is no reason to assume that anyone is relying on it.

## Regressions

There will unfortunately always be bugs that accidentally cause backwards compatibility to be broken.
These are called *regressions*: Something that used to work no longer does.

Regressions are not strictly limited to documented behavior:
The documentation is not a practically complete specification,
so exceptions are commonly made, where the engine developers work to preserve a previous behavior even if it was undocumented.
Depending on the impact, regressions concerning undocumented behavior
may also be considered important enough to be fixed.

{{< notice tip >}}
If you find out that behavior unexpectedly changed, please
[file a bug report](https://github.com/luanti-org/luanti/issues/new?labels=Unconfirmed%20bug&template=bug_report.yaml).
{{< /notice >}}

## Client and Server

You need to decide whether older clients not supporting newer features
is game-breaking and older clients have to be prevented from playing,
or whether you can implement sensible fallbacks.

When writing a game you can provide an appropriate default value for `protocol_version_min`
in `minetest.conf` to facilitate this.

{{< notice tip >}}
You can use `core.get_player_information` to get protocol versions and `core.protocol_versions`
to relate them to Luanti client versions. For example, to check whether a client
has at least the feature set of Luanti 5.8.0 or newer, you could do:
`core.get_player_information(player_name).protocol_version >= core.protocol_versions["5.8.0"]`
{{< /notice >}}

## Lua API

"Namespaces" (like `core`), "classes" (like `VoxelArea`) and "structs"
(like the return value of `player:get_player_control()`)
can and will be extended by adding new constants, functions or methods.
Generally, as a rule of thumb, "tables may have more fields in the future".

Maybe there is no `core.frobnicate` today, but there might be tomorrow.
This is why you should use your own namespaces and wrappers
and not try to "extend" engine namespaces or classes yourself -
there could be collisions in the future. [^maintenance]
If you are forced to share a namespace with the engine (e.g. in entity definitions or item definitions),
the current convention is to prefix your fields with an underscore (`_`);
if multiple mods share the same namespace, using the mod name for namespacing is recommended.

[^maintenance]: And additionally, even if the risk of this is low,
you make it harder for maintainers to see where something comes from.
A maintainer seeing `core.foo(...)` will expect it to be a Luanti function,
documented in the Luanti documentation, not something coming from some (which?) mod.

Similarly, functions that return tables may return tables with *more* fields in the future.
Hence you **should not** iterate over fields and raise errors for fields you don't recognize:
Just get the fields you want and ignore extraneous fields.

Functions that take tables might support more fields in the future.
Hence you **should not** provide extraneous fields in your tables
(it's also bad style and likely confusing to a reader).

Functions that take multiple parameters might take more parameters in the future.
Be warned that not all engine functions are designed to be overridden ("hookable").

{{< notice tip >}}
If you're hooking a function,
you should use a vararg to perfectly forward the parameter list, like this:

```lua
local frobnicate = core.frobnicate
function core.frobnicate(...)
	print("frobnication is starting! arguments are:", ...)
	return frobnicate(...)
end
```

This also applies conversely to perfectly forwarding return values.
This might require storing them in a table:

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
{{< /notice >}}

Functions that take values of certain types today may take values of different types tomorrow.
For example it often happens that a parameter list is replaced with a single table argument.

## See also

- [Keeping world compatibility (as a creator)](/for-creators/keeping-world-compatibility/)
- [Compatibility guarantee for players](/for-players/compatibility)
