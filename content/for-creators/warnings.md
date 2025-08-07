---
title: Warnings
---

# Warnings

When you run Luanti, it often produces helpful warnings
that indicate issues with mods and games that need to be addressed.
Most commonly, these are *deprecation warnings* for APIs that
will change in a future Luanti version.
This document explains some of the more frequently encountered warnings and how to resolve them.

## Log levels

As a mod developer, you might want to set `chat_log_level = warning` to see client warnings right in your chat.

Your `debug_log_level` should be at least `warning`; you can then find warnings in `debug.txt`.

If you run Luanti in a terminal, you should see warnings in its output.

Note that stack traces are logged at `info` log level.

## Global variable access

Using a static analysis tool like [luacheck](https://github.com/lunarmodules/luacheck/)
to catch mistakes related to global variables *before the broken code is run* is strongly recommended.
Luanti also has its own rudimentary warnings for some basic yet frequent mistakes *at runtime*:

> Undeclared global variable "..." accessed at ...

You are trying to access a global variable which does not exist.
This is likely a bug: The intent is to access a (local) variable, but instead you get `nil`,
for example because you mistyped the variable, the variable declaration was moved,
or the assignment to a global variable was moved.

```lua
local bar = 42
...
local foo = baz -- typo! meant bar!
print(foo) -- unexpected nil
```

If you want to check whether a global variable does exist, for example for a mod you're optionally depending on,
you can use either `rawget(_G, "name")` or `core.global_exists("name")`.
After you have done the check, you can simply access the global variable, though you may want to localize it instead:

```lua
-- at the top of the file:
local my_opt_dep = rawget(_G, "my_opt_dep")
...
-- somewhere deep in the remaining code:
if my_opt_dep then my_opt_dep.frobnicate(...) end
```

> Assignment to undeclared global variable "..."

You are assigning a value to a global variable which was not assigned at load time.
This usually happens when you forgot to make a variable `local`:

```lua
local function frognicate(...)
	pi = 3.14 -- oops! should be local!
end
```

This is typically just a minor code quality issue.
However it is likely to lead to bugs, especially if this happens in multiple places:
You may overwrite another mod's global variables accidentally;
recursive functions may overwrite what ought to be local variables.

Instead, you should use a local variable.
In general, mods should avoid polluting global variables.
If your mod needs to expose an API, it is recommended to use a single global variable named after the mod.

## Media files

> Server: ignoring file as it has disallowed characters: "`<filename>`"

A file is not being considered as a media file as it contains disallowed characters.
You can use `find -name <filename>` to find the path of the offending file.

Your media files should only use the following characters in their name:

* ASCII letters (`a`-`z`, `A`-`Z`)
* ASCII digits (`0`-`9`)
* Underscore (`_`)
* Hyphen (`-`)
* Period (`.`)

The fix is simply to rename or, if obsolete, remove the file.

By convention, your media file names should typically start with the mod name followed by an underscore.
This avoids conflicts and makes it clear which mod a file belongs to.

### PNG

> iCCP: known incorrect sRGB profile

This warning concerning color profiles is produced by libpng.
How to fix it is discussed e.g. [on StackOverflow](https://stackoverflow.com/questions/22745076/libpng-warning-iccp-known-incorrect-srgb-profile).
Re-exporting images with a proper exporter, e.g. using ImageMagick's `mogrify`, fixes the files.

> Interlace handling should be turned on when using `png_read_image`

[Adam7 interlacing](https://en.wikipedia.org/wiki/Adam7_algorithm)
lets someone who has only partially received an image see a degraded version of it.
Because Luanti only loads images once they have been fully received,
and interlacing typically increases file size, there is no point at all in using interlacing.

Export your images without interlacing.
Make sure that the "Interlacing (Adam7)" checkbox is unchecked when exporting PNGs from GIMP.

### glTF

> embedded images are not supported

Your mesh contains embedded images which will be ignored by Luanti.
This unnecessarily increases file size and might confuse people who work with the model.

Textures are supplied via the `textures` property (for entities) or the `tiles` node definition field (for nodes).

If you're already using a different texture or after you have extracted the texture, you can simply strip images.
Both extracting and stripping images can be done using [gltfutil](https://github.com/luanti-org/modtools).
(Since `.gltf` files are just JSON, they are also fairly easy to edit manually.
In this case, you need to remove the `"images"` property and remove the `"source"` property from all textures.)

Note that URI references to external images are not supported by Luanti
and hence should not appear in your glTF files either.

> multiple animations are not supported

The mesh contains multiple glTF animations ("timelines"),
but Luanti only supports a single timeline and ignores all timelines except for the first one.

You need to batch all animations into a single one and use frame ranges within this animation.

See also [Using Blender](/for-creators/models/using-blender/).

> negative weights

The mesh contains negative vertex weights which were ignored.
The affected weights should be set to zero (or outright removed via some other means) instead.

> nodes using matrix transforms must not be animated

A node uses a matrix transformation, yet there is an animation channel targeting this node.
This is problematic because the TRS (translation, rotation, scale) decomposition of the matrix need not exist,
and even if it does exist, need not be unique (e.g. half-turns are equivalent to inverting an axis).

For reasons like these, glTF does not allow animated nodes to use matrix transforms.
Luanti will warn about this and ignore the animation.

If you do want to animate the node, decompose the matrix into TRS properties yourself.
Otherwise simply remove the animation channel targeting the node.

## Deprecation warnings

Luanti typically logs deprecation warnings if you use the API in a way that will stop working in a future version.
Make sure to check the "compatibility notes" section of the relevant changelog.

If you want to continue supporting older Luanti versions,
you might have to check for the existence of an API and fall back to using the legacy API if necessary.

> Mod ... at ...:
> Mods not having a `mod.conf` file with the name is deprecated.
> `depends.txt` is deprecated, please use `mod.conf` instead.
> `description.txt` is deprecated, please use `mod.conf` instead.

Very old Luanti versions used to expect mod metadata in a bunch of separate files.
The mod name was taken to be simply the folder name.
(This sometimes caused problems when users downloaded mods and got a folder with a different name, e.g. `mod-master`.)

Newer Luanti versions expect mod metadata, including the mod name, in a `mod.conf` file.

If `depends.txt` looked like this:

```
my_dep_1
my_dep_2
my_opt_dep_1?
my_opt_dep_2?
```

you would create a `mod.conf` in the mod folder with the following contents:

```
name = <folder name>
depends = my_dep_1, my_dep_2
optional_depends = my_opt_dep_1, my_opt_dep_2
description = <contents of description.txt>
```

> Field "use_texture_alpha" on node ...: Boolean values are deprecated; use the new choices

Luanti now gives more fine-grained control over node `use_texture_alpha`.
If your node is supposed to be opaque (`use_texture_alpha = false`), you should use `use_texture_alpha = "opaque"`.
This enables some optimizations and makes cheating with texture packs harder.
If your node is supposed to have transparent areas, but each pixel is either fully opaque or fully transparent,
use `use_texture_alpha = "clip"`.
Only if your node needs semitransparent areas should you pick `use_texture_alpha = "blend"`.
Always using `use_texture_alpha = "blend"` may have disastrous consequences for rendering performance.

> Field "image" on TileDef: Deprecated: new name is "name".

You have a node tile table with `{image = "...", ...}`; it should be `{name = "...", ...}` instead.

> Reading initial object properties directly from an entity definition is deprecated,
> move it to the 'initial_properties' table instead. (Property 'prop' in entity '...')

This happens if your code looks something like this:

```lua
core.register_entity("my_mod:my_ent", {
	prop = ..., -- initial property right in the entity definition table :(
	...,
})
```

which means that `self.prop` will evaluate to the *initial* property;
modifying `self.prop` will modify the common *initial* property for everyone.
This is a frequent source of mistakes.

If you want to work with per-instance properties, you need to use
`self.object:set_properties({prop = ...})` for updates,
and `self.object:get_properties().prop` to query them
(for performance reasons, you might want to cache this).

Your entity definition should look like this:

```lua
core.register_entity("my_mod:my_ent", {
	initial_properties = {
		prop = ..., -- initial property in the proper table :)
		...,
	},
	...
})
```

If you happened to access initial properties via `self.prop` or `entity_def.prop`,
you of course now need to access them as
`self.initial_properties.prop` resp. `entity_def.initial_properties.prop` instead.

> Deprecated call to `set_bone_position`, use `set_bone_override` instead

Do what it says and switch to the new API.
Be careful: This is not just a rename, `set_bone_override` expects its arguments in a slightly different format.

> Deprecated call to `get_attribute`, use MetaDataRef methods instead.

Instead of `player:get_attribute(name)`, use `player:get_meta():get(name)`.

> Deprecated call to `set_attribute`, use MetaDataRef methods instead.

Instead of `player:set_attribute(name, value)`, use `player:get_meta():set_string(name, value)`.

> Calling `get_connected_players()` at mod load time is deprecated

Such a call is pointless as it will always be `{}`.
Doing anything "for each connected player" at load time simply makes no sense:
There are no connected players.

This warning likely requires eliminating some dead code,
and possibly a bit of refactoring, to resolve properly.
Often, it might makes sense to postpone the call to the first server step
via `core.after(0, my_mod_step)`.
(A common cause is that some mod "step" function is run initially at mod load time and iterates over connected players.)
