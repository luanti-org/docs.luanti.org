---
title: Modding Tips
aliases:
  - /Modding_Tips
  - /modding-tips
  - /content-dev/modding-tips
---

This page is a random collection of various tips and tricks, and solutions to common tasks/problems that occur frequently in mod/game development. See also: [Modding FAQ](/about/faq/#game-and-mod-development)

## Troubleshooting

### OOM errors

If your game or mod crashes due to an OOM error, it means that Luanti has ran out of memory (=OOM). This happens if your Lua code somewhere uses up a large amount of memory. This can happen if you construct large tables and fill them with lots of data and never free them, or you got a nice [memory leak](https://en.wikipedia.org/wiki/Memory_leak). Badly coded mapgens are a likely cause of OOM errors, if you implement a Lua API, you should always apply all [Mapgen memory optimizations](/for-creators/mapgen/memory-optimizations).

### “Irrlicht: PNG warning: iCCP: known incorrect sRGB profile”

If you debug log level is set to “warning” or lower, you probably ran into the warning “Irrlicht: PNG warning: iCCP: known incorrect sRGB profile” quite often.

```
it means that *some* PNG file is bad. But this warning is useless for you because there's no file name mentioned.

```

If you want to track down _which_ PNGs are broken, set the debug level (`debug_log_level`) to “verbose”. Now the log will show the PNG file name right above the warning. Now just fix the offending PNGs with your favorite image editor to get rid of these warnings.

### Deprecated function calls

As Luanti develops, some API functions fall out of use and become deprecated in favour of newer functions with different names. Looking at console log or debug log output should give you the deprecation warnings that your mod throws. See the warning, which might give the name of the new function, and the Lua API for any eventual change in behavior it has.

When upgrading your mod from version 0.4.x to 5.x, you may use the [MT-replace-deprecated.sh](https://gist.github.com/SmallJoker/cb89c3f9e4be27a0e8bc10ced1c5fc31) script ([forum thread](https://forum.luanti.org/viewtopic.php?f=18&t=20403)) which can automatically rename some deprecated functions.

To abort on any execution of a deprecated function, you can set the “deprecated_lua_api_handling” setting to “error”, useful during development to clearly see any deprecated functions being used.

### Positional sounds are either full volume or completely silent

Problem: You want to play a sound at a position, but it's always at full volume when you're in range or completely silent if you're not. For example, you have a zombie that growls, but when it growls, somehow its growl is either at full volume or not hearable at all (if you're out of the sound range); there's no gradient at all. This can be annoying.

Reason: Your audio file is probably stereo. The fix is to make all positional sounds mono. You probably want to check ALL audio files then, it's very likely this wasn't the only one.

The documentation says:

```
For positional playing of sounds, only single-channel (mono) files are
supported. Otherwise OpenAL will play them non-positionally.

```

Note that non-positional sounds do not have to be mono, so you don't have to touch them.

### The mapgen generates some nodes that let me see through the world but I still collide and/or interact with them

This can happen in the mapgen v6 if you forgot to set some mapgen aliases, or a mapgen alias is set to a non-existing node. What is happening here is that these “broken” nodes are [ignore](/for-players/nodes/#ignore) nodes because the v6 mapgen doesn't know which node to place here (because the given node name was invalid or undefined).

If you are using the mapgen v6, you must make sure that the mapgen aliases are set to a valid value. See `lua_api.md` for a list.

### The lighting of my node is all-dark / messed up

This is probably because they don't have `paramtype="light"` set. Some node drawtypes like "mesh" require it for the lighting to work correctly (see `lua_api.md` for details). If this case applies to you, you need to add `paramtype="light"`.

Note that updating the node definition will not immediately fix nodes in existing worlds, the lighting of those nodes needs to be updated as well. This can be done by calling the `/fixlight` command

### I have floating plants

If you have plant decorations that for some inexplicable reason float in the air, there are multiple possible reasons for this, but here are some of the common ones:

- The v6 mapgen is a little buggy
- Other decorations accidentally have plants on the floor and the schematic author overlooked them when saving the schematic
- ???

## Tips

### Making warnings visible in chat

By default, warnings are only seen in the debug log file and the program's console. For developers, it is very helpful to make all warnings visible in the chat as well so you won't miss them. Many warnings are about stuff like deprecated code, so it's a good idea to have them always highly visible.

Set the setting `chat_log_level` to `warning` to enable this.

### Improving Lua mapgen memory performance

If you created a Lua mapgen, it is strongly recommended to make sure you optimize the memory performance, otherwise you could quickly run into OOM (out-of-memory) errors. See [Mapgen memory optimizations](/for-creators/mapgen/memory-optimizations).

### More optimization tips

For more optimization tips, go to [Lua Optimization Tips](/Lua_Optimization_Tips).

### Checking if a mod exists

If you want to check if a mod named “example” is currently loaded and actively used, use this code:

`local mod_exists = minetest.get_modpath("example") ~= nil`

Now `mod_exists` will be true if the mod exists and false otherwise.

This works because `minetest.get_modpath` returns nil if the mod is not loaded.

### Ensuring mod interoperability

A good mod is one that still works when a lot of other mods are used. Read [Mod_interoperability](/Mod_interoperability) for a lot of useful hints.

## Quality checklist

Here's a list of things to check in your game or mod to improve general quality and to avoid common pitfalls. Not all things might apply for your game, always use good judgement.

Note: Some of these can be checked quickly with [QA-Block](https://content.luanti.org/packages/bell07/qa_block/). See also: [Development Tools](/Development_Tools).

### Preventing crashes, exploits and bugs

- In `minetest.after`, do you consider that any external variable or object might become nil or disappear in the meantime?[^1]
- In formspecs, did you check for all [Time Of Check is not Time Of Use vulnerabilities](https://forum.luanti.org/viewtopic.php?f=47&t=19129)?
- In formspecs, do you [Never Trust User-Provided Data](https://forum.luanti.org/viewtopic.php?f=47&t=19129)?
- In formspecs, do you enclose all variable, unpredictable text in `minetest.formspec_escape`? [^2]
- Does the game behave properly when restarted?[^3]
- Did you check if implementing [mod security](https://forum.luanti.org/viewtopic.php?f=18&t=12471) is necessary? If yes, did you implement it?
- For singleplayer games: Do you error out if someone tries to run your game in multiplayer?[^4]
- Do you restrict detached inventories to players, where necessary?[^5]

### General code quality

- Does the game/mod avoid polluting the global namespace with tons of identifiers?[^6]
- All deprecated code removed/replaced?[^7]
- Are all APIs (that are intended for external use) documented?

### Performance

- Do you use the “buffer” argument in the LuaVoxelManip and Perlin noise functions (like `get_data` or `get_2d_map_flat`)?[^8]
- Do you avoid re-creating the same Perlin noise over and over again?[^9]

### Entities/players

- Do all entities still behave properly when they unload, and then load again?[^10]
- If you change player physics (`set_physics_override`) anywhere, will it still work when another mod changes it?[^11]

### Items and nodes

- Is `is_ground_content` set to false for all nodes that the cavegen should not destroy?[^12]
- Do all nodes have appropriate sounds?[^13]
- Do all nodes have appropriate selection boxes?[^14]
- Do all items intended for use only in Creative Inventory have set the `not_in_creative_inventory=1` group?
- Do all crafts work?[^15]

### Game/mod metadata

- For games: Are all unsupported mapgens disabled in game.conf?
- Does your game/mod have a name?
- Is the name of the game/mod used consistently everywhere?
- For games: Name, icon, header present?[^16]

### Graphics, audio, text

- Are all positional sounds mono?[^17]
- Does the UI still work in higher scaling/DPI?
- Are all required textures included?
- Do your leaves still look okay when simple leaves are used (`leaves_style = simple`)[^18]
- Do all items that the player can legitimately get have `description` set?
- Do all chat commands have `description` and parameter list set?
- In the parameter list of chat commands, does it follow the format defined in `lua_api.md`?
- Do all items have unique descriptions (=no confusing duplicates, unless intentional for some reason)
- Is the writing style consistent across the entire game?
- No typos/grammar fails?
- Do you use `blank.png` instead of a custom transparent image?[^19]
- Do all items have an appropriate item image that helps in keeping items apart?[^20]

### Translations

- Does the UI provide reasonably enough space for translations?
- Was at least one translation play-tested?
- Do you avoid using string concatenation to include variable text?[^21]
- Can all user-facing texts be translated?
- Have all in-game images with baked-in, untranslatable texts, been replaced with text?[^22]

## Footnotes

[^1]: For example, if minetest.after does anything on a player object that you got 5 seconds ago, it's possible the player has left in that time. If you do anything (besides checking for existence) on the player object, you trigger a crash

[^2]: Variable text, especially translations or user input, can potentially contain the magic characters that formspecs use, so if unescaped, they will break the formspec.

[^3]: Some game data might be accidentally reset or just not be persisted across restarts. For example, rainy weather might be reset to clear because it was not saved.

[^4]: Use `minetest.is_singleplayer` to check. Use Lua's `error` function to error out, or just kick all players.

[^5]: Detached inventories are sent to everyone unless you specify a name in the registration. So other players using a modified/hacked client could theoretically alter any detached inventory without an attached name.

[^6]: The keyword “`local`” should be your new friend. Everything that does not need to be visible outside should be made local. This will avoid a lot of weird bugs caused by mods overwriting their global variables each other. As a rule of thumb, your mod should only have up to 1 global variable which is also the same name as the mod. Make this a table in which you include all global stuff. You can use [QA-Block](https://content.luanti.org/packages/bell07/qa_block/) to find suspicious global variables.

[^7]: See also: [MT-replace-deprecated.sh](/MT-replace-deprecated.sh)

[^8]: See also [Mapgen memory optimizations](/for-creators/mapgen/memory-optimizations)

[^9]: See also [Mapgen memory optimizations](/for-creators/mapgen/memory-optimizations)

[^10]: Entities will forget most variables when they unload, which is easy to overlook for beginners. Make sure to make use of staticdata.

[^11]: If you don't apply any checks when overwriting player physics, this will very likely lead to very hilarious bugs if 2 mods want to change player physics directly, as they will constantly compete for “their” physics. This will likely screw up the player physics badly. To solve this, you should generally avoid setting player physics directly, unless you want to implement a physics interface yourselves. But for normal use, we highly recommend to use an [Mod_interoperability#Player_physics API](/Mod_interoperability#Player_physics_API).

[^12]: Note this value is true by default, and can be forgotten easily.

[^13]: Of course, silence is also “appropriate” if that's what you intended.

[^14]: As a rule of thumb, try to match the graphics pixel-perfectly, if it makes sense. Selection boxes that are completely misplaced or just tiny are generally perceived as highly annoying by players.

[^15]: Use a craft guide to check

[^16]: The images just to help identifying the game in the game icon list in the main menu. Without an icon, your game is harder to find. At least draw a dummy image if you're in a hurry.

[^17]: See troubleshooting above.

[^18]: Luanti will use the color of the transparent pixels of the PNG file; check out your manual of your graphics program to learn how to set the color of transparent pixels

[^19]: blank.png is a fully transparent texture that is available by default. No need to create your own transparent texture.

[^20]: It's annoying if 2 very different items share the same icon

[^21]: If you don't understand this, review lua_api.md

[^22]: There might be possible exceptions, such as names and logos. But generally, all baked-in text should be avoided where possible.
