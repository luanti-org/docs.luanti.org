---
title: Changelog
aliases:
  - /Changelog
  - /OldChangelog
  - /changelog
---

# Changelog

Note that not all changes made to the code between releases are listed here. Fixes to bugs that were introduced after the previous release, small internal changes, code style fixes, and changes of the like are not listed. If you want a list of _every_ change made between releases see the [commit log](https://github.com/luanti-org/luanti/commits/master).

## 5.13.0 → 5.14.0

Released on 5 October 2025.

### Deprecations and compatibility notes
- Removed broken interlaced 3D rendering mode (`3d_mode = interlaced`) (_sfan5_)

### Client / Audiovisuals
- Android: open input dialog on double tap with physical keyboard (_siliconsniffer_)
- Speed up of texture loading (by caching search paths) (_sfan5_)
- The minimap now correctly shows the color of overlay tiles (_sfan5_)
- 3D models: Interpolated bone positions do work again (_appgurueu_)
  - Fixed a minor memory leak (_appgurueu_)
- World buttons are no longer shown when no world is selected (_birdlover32767_)
- Text inputs now handle Ctrl+Shift+Left/Right inputs for text selection (_SmallJoker_)
- Chat messages and server connections are now logged (_Sokomine_, _sfan5_)

### World / Server / Environment
- `core.parse_coordinates` now returns a `vector` object (_sfan5_)
- Supported backends are shown in the `--help` output. (_sfan5_)
- Reliability improvement on mapblock loading/unloading during generation (_Montandalar_)
- Node dig particles are sent to all other players. (_sfan5_)
- Improved networking for batch particle sending (_appgurueu_)

### Script API / Modding
- The callback `on_timer(...)` now provides node and timeout information. (_sfan5_)
- Add back the missing endElement in parseTag for `<action>` (_Emojigit_)
- Allow floating-point numbers for `HTTPRequest.timeout` (_whosit_)
- Add API to cancel async jobs (_y5nw_)
- Additional control in `core.dynamic_add_media(...)` to hint client to save media to the cache (_sfan5_)
- Improvements to the mod profiler, Capture Tracy zones (_DS_)
- Add `exclude_player` (filter) to `ParticleSpawner` (_sfan5_)
- Add `core.strip_escapes(...)` (_sfan5_)
- `core.generate_decorations(...)` may now use the correct biome map (_repetitivestrain_)
- New feature to scale nametags by distance (_sfan5_)

### Misc / Maintenance
- Build system maintenance (_sfan5_, _rubenwardy_, _AFCMS_, _ProunceDev_)
- Lua API improvements (_sfan5_, _cx384_, _Zughy_)
- Code cleanup and bugfixes (_sfan5_, _DS_, _SmallJoker_, _y5nw_)
- Memory optimization of "empty" blocks (_lhofhansl_)
- Additional safety checks (_sfan5_, _SmallJoker_)


## 5.12.0 → 5.13.0

Released on 1 August 2025.

### Deprecations and compatibility notes
- Vectors passed to C++ API functions may no longer have `nil` components.
  - This change can reveal logic issues within mods.

### Client / Audiovisuals
- 9-slice formspec image buttons now display correctly with `gui_scaling_filter` (_Krock_)
- Fixed an issue where main menu dialogs could overlap upon start (_grorp_)
- Tabs in formspecs can now be changed using Ctrl(+Shift)+Tab (_siliconsniffer_)
- The formspec element `model[]` now supports floating-point frames (_appgurueu_)
- The minimap now correctly displays `drawtype = "air"` nodes (_Xeno333_)
- Main menu ContentDB usability improvements (_grorp_)
- Meshgen (i.e. visible mapblocks) code cleanups and improvements (_sfan5_)
- Add keybinding for world close key (`keymap_close_world`) (_DragonWrangler1_)
- Relaxed path checks in the main menu to allow downloading mods to symlinked directories (_sfan5_)

### World / Server / Environment
- The Luanti version is now printed after the ASCII logo (_sfan5_)
- Fix main menu hang caused by dropped DNS packets (_sfan5_)
- Fix newline conversion in formspecs on Windows (copy & paste) (_PtiLuky_)
- Fix crash caused by empty particle spawner texture (_Krock_)

### Script API / Modding
- Various lua_api.md improvements (_grorp_, _jordan4ibanez_, _Xeno333_, _TheEt1234_)
- Empty Voxel Manipulators can be created using `VoxelManip:initialize()` (_sfan5_)
- Fixed an issue where sizeless text input formspecs would incorrectly send `ExitButton` as text (_Krock_)
- `core.show_formspec` now allows showing a player inventory (_Krock_)
- `core.get_node_raw` is now a public API (_sfan5_)
- New API function `core.get_mapgen_chunksize()` (_sfan5_)
- Newly spawned entities now have a persistent GUID, see `ObjectRef:get_guid()` (_sfence_)

### Misc / Maintenance
- Many various code improvements and cleanups (_sfan5_, _PtiLuky_)
- Improved unit tests (_sfan5_, _lhofhansl_)
- Model skeleton animation cleanups, test and documentation (_appgurueu_)
- Header files are now added as CMake sources (useful for Visual Studio (Code)) (_PtiLuky_)


## 5.11.0 → 5.12.0

Released on 23 May 2025.

### Deprecations and compatibility notes
- 5.12.0 is the first build to use SDL2 for window and input handling
  - Key bindings are now based on scancodes (i.e. key positions) instead of keycodes ("marking on the keycap"). After updating Luanti, the user will be prompted to verify and/or reassign their key bindings. See also: [Controls](/for-players/controls)
- Saved worlds that were newly created (or migrated using `luanti(server) (--server) --migrate ... --world ...`) by 5.12.0 can no longer be loaded by older versions. Worlds that are played over the network are unaffected.
  - Details: instead of a unified `pos` field for `x`, `y` and `z`, the coordinates are now saved individually. This makes the database more accessible by external tools. ([even more details](https://github.com/luanti-org/luanti/pull/15768))
- Lua API: The node/item registration functions now use stricter checks to avoid bad practices in mods.
- Lua API: "Perlin noise" (and related functions) were renamed to "Fractal value noise". The old API remains accessible, yet without any deprecation warnings.
  - The old name was inappropriate: Luanti has always implemented value noise, *not* perlin noise.
- Lua API: The tool capabilities of the optional `"hand"` inventory slot now entirely overwrite the default hand. For clients < 5.12.0, default groupcaps were not overwritten by the `"hand"` slot.
- Remote media: The client will now use GET requests to allow static media hosting. ([details, writeup](https://github.com/luanti-org/luanti/pull/15885#issuecomment-2708510220))

### Client / Audiovisuals
- Fix some models not rendering correctly as nodes (_appgurueu_)
- Android: Fixed a bug where fonts could turn black when leaving a world while the app is running in the background (_grorp_)
- Change of the default appearance: buttons now have a flat appearance (_siliconsniffer_)
- Improved caching for generated textures (speed-up) (_sfan5_)
- Closing dialogues from the pause menu now shows the pause menu again (_SmallJoker_)
- Better texture filtering handling to avoid blurriness (_sfan5_)
- The keybinding dialogue is now implemented in Lua and can be found in the Settings dialogue (_y5nw_)
- Main menu: The SDL version is now shown in the "About" tab (_y5nw_)
- Main menu: new "Reviews" tab for ContentDB entries (_rubenwardy_)
- The inventory now animates animated nodes (_cx385_)
- Many (SDL) input handling changes (_y5nw_)
- Dynamic media is again properly cached on client-side (_ashtrayoz_)
- Android: new interaction modes (camera movement/dig/punch), changeable by settings (_grorp_)
- Z-fighting fixes for certain node (or tile overlay) combinations (_sfan5_)
- `Sneak` and `Aux1` toggle mode setting (_maplemedley_)
- Removed broken fall bobbing effect (_sfan5_)
- New Android setting defaults related to the view range (_sfan5_)
- Fixed fallback fonts not being used when the server provided custom fonts (_y5nw_)
- Fixes dynamic shadows performance regression (_sfan5_)
  - This can be tweaked using the setting `performance_tradeoffs`.
- Improved main menu server list behavior (_siliconsniffer_)

### World / Server / Environment
- Improved mapblock load/save performance (_Desour_)
- More (performance-) efficient container for (Lua) object tracking (_appgurueu_)
- Server texture packs are now documented in `doc/texture_packs.md` (_cx385_)
- Reduced network traffic for some pre-join data for clients >= 5.12.0 (_sfan5_)
- `/msg` now returns an echo of the sent contents (_GreenXenith_)
- Decoration placement performance optimization (_kno10_)
- IPv6 is now enabled by default (_sfan5_)
- More accurate collision code (_kno10_)
- `load_mod_* = false` lines are no longer written to `world.mt` (_appgurueu_)

### Script API / Modding
- glTF models: Local transforms no longer affect a skinned mesh. This bug used to cause wrong rendering. (_appgurueu_)
  - Tip: To avoid the bug on older Luanti client versions, simply remove the offending local transforms.
  - You can use [glTF validator](https://github.khronos.org/glTF-Validator/) to check whether your models are affected. The `NODE_SKINNED_MESH_LOCAL_TRANSFORMS` warning should not appear.
- Fix `visual_scale` not applying to skeletally animated meshes when used for nodes (_appgurueu_)
- New formspec element `allow_close[<bool>]` to prevent formspecs from closing (_v-rob_)
  - This only works for clients >= 5.12.0
- Various API documentation improvements (_cx384_, _grorp_, _Desour_)
- Fixed LBMs sometimes not being executed on newly generated mapblocks (_sfan5_)
- New function `table.copy_with_metatables` (_appgurueu_)
- `core.item_drop` now returns the spawned `obj` (_SmallJoker_)
- New `visual = "node"` option for entities to make it look like a regular node. (_sfan5_)
  - Clients prior to 5.12.0-dev cannot see this entity.
  - The builtin falling node entity was converted too.
- `InvRef:remove_item` now allows filtering by metadata (_andriyndev_)
- Basic API to restrict the available camera modes (1st, 3rd back, 3rd front) (_sfan5_)

### Misc / Maintenance
- MacOS now uses SDL2 by too by default (_sfence_)
- Various mapblock-related optimizations (_lhofhansl_)
- Many fixes and improvements for the OpenGL code (_sfan5_)
- Server/Client settings are now annotated more clearly (_grorp_)
- Other error/crash fixes (_deveee_, _sfan5_)
- Various build fixes and updates (many contributors)
- Unittest additions (_appgurueu_, _sfan5_, _kno10_)
- SDL input corrections, fixes (_Desour_)
- Various code cleanups and corrections (_appgurueu_, _sfan5_, _cx385_)

## 5.10.0 → 5.11.0

Released on 14th February 2025.

### Deprecations and compatibility notes

- Clients >= 5.11.0 will no longer support BMP texture files. This deprecation was first announced in 5.8.0.
- Basic shaders support is now mandatory. New minimum required OpenGL version: 2.0.
  - See [https://github.com/luanti-org/luanti/issues/15370](https://github.com/luanti-org/luanti/issues/15370) for details.
- A bug related to skeletal animation has been fixed. The bug occurs when bones have "perfect" 180° rotations, equivalent to negatively scaling two axes.
  If these bones are then animated via bone overrides, the overridden rotation will incorrectly appear to be relative to the 180° rotation.
  This may result in wrong bone rotations for mods that rely on this bug. These mods need to fix the rotations passed to `set_bone_override` / the deprecated `set_bone_position`.
  Multiple workarounds are possible to ensure consistency with older Luanti clients which exhibit the bug,
  such as editing models to not have perfect rotations, overriding bone scale to force newer clients to replicate the bug,
  or overriding bone scale to work around the bug on older clients which support setting scale.
  See [the issue](https://github.com/luanti-org/luanti/issues/15692) for details.

### Client / Audiovisuals

- Fix shadow flicker on camera offset update (_sfan5_)
- In-game settings menu (_grorp_)
- Add chat console scrollbar (_chmodsayshello_)
- Fix occasional z-fighting rendering issues by overlay textures (_sfan5_)
- Fix MSAA and bloom flashing artifacts (_lhofhansl_)
- Main menu additions
  - Add mods button (_cx384_)
  - Add server url button (_siliconsniffer_)
  - Add server favorite button (_siliconsniffer_)
  - Player list for public servers (_siliconsniffer_)
- Fix bloom with `post_processing_texture_bits` < 16 (_sfan5_)
- Fix distorted Sun, Moon and Star visuals based on Orbit Tilt (_veprogames_)
- New setting `transparency_sorting_group_by_buffers`. This may improve performance at the cost of visual issues. (_Desour_)
- Implement an editor to customize the touchscreen controls (_grorp_)
  - This screen can be opened though the pause/exit menu.
- Support FSAA in combination with post-processing (_grorp_)
- Support `video_driver = opengl3` for non-SDL builds (_Krock_)
- Rendering-related fixes (_grorp_, _sfan5_)

### World / Server / Environment

- Fix incorrect/outdated player inventory contents after failed "drop" action (_Krock_)
- Fix clients sometimes erroneously disconnecting after 30 seconds (_sfan5_)
- Restore proper rollback database indexing (_appgurueu_)

### Script API / Modding

- `core.protocol_versions` for easier engine version lookups (_rollerozxa_, _appgurueu_, _sfan5_)
- Allow overriding fonts via media files (_appgurueu_)
- New in-game debug view: highlight bounding boxes (_sfan5_)
- New function `core.spawn_tree_on_vmanip` (_cx384_)
- Fixed fruit placement regression in L-system trees (_cx384_)
- Continued work on the glTF model reader (_appgurueu_)
  - STEP (function) interpolation is now supported.
- New function `core.settings:get_pos`. This deprecates `core.setting_get_pos`. (_AFCMS_)
- Add particle blend mode "clip" (_appgurueu_)
- Documentation improvements (_kno10_, _luk3yx_, _Wuzzy_, _cs384_, _fineless71_)
- Invalid `ore_type` values in `core.register_ore` now result in a proper error (_cx384_)
- Add optional relative weight parameter to biomes (_kno10_)

### Misc / Maintenance

- Improve sleep accuracy on FPS limiter (_Hanicef_)
  - This mainly affects Windows clients.
- Texture filters/generation speed improvement (_sfan5_)
- Performance improvements in rendering (world and formspec) (_sfan5_, _Desour_, _appgurueu_)
  - New setting `mesh_buffer_min_vertices` for fine-tuning.
- Crash fixes (_appgurueu_, _Krock_)
- Replaced minetest.net with luanti.org in several places (_veprogames_)
- Implement script sandboxing for main menu (_sfan5_) + follow-up bugfixes (_sfan5_)
- Code cleanups and minor fixes (_grorp_, _sfan5_, _appgurueu_)

## 5.9.1 → 5.10.0

Released on 10 November 2024.

### Deprecations and compatibility notes

- **Minetest is now called Luanti.**
  - The executable name has changed, thus custom links and scripts might need maintenance.
  - Any `minetest.conf` files remain compatible (naming yet not changed).
  - For Unix: `$HOME/.minetest/` remains used for system-wide installations (naming yet not changed).
  - Mods may use the `core` table instead of the equivalent `minetest`. (see lua_api.md)
- OpenGL ES 1.0 (ancient) is no longer supported. OpenGL ES 2.0 will be used instead.
- (basic) shaders will eventually become a requirement. A warning is shown if they are disabled.

### Client / Audiovisuals

- Added a workaround to fix shader errors on buggy Intel iGPU drivers on Windows 8.1 and older (_sfan5_)
- Fixed Post Processing with the `ogles2` driver (_grorp_)
- The main menu server filter / search box now filters more intuitively (_appgurueu_)
- Touch controls are now enabled/disabled based on the used input device (_grorp_)
- Main menu: redesigned ContentDB tab (_rubenwardy_)
- Improved texture pack compatibility when using the `[mask` modifier (_SmallJoker_)
- More fancy shaders (_GefullteTaubenbrust2_)
  - Server-controlled bloom shader API (_grorp_)
  - Follow-up fixes (various contributors)
- Fixed sound mute on macOS (_sfence_)
- Improved formspec scaling (_grorp_)
- The minimap is again scaled proportional to the screen size (_grorp_)
- Improved distinction of touch-related settings (_grorp_)
  - UI scaling settings and touch input are no longer tightly tied together (_okias_)
- Transparency sorting now works more reliably (_Desour_)
  - Can be disabled entirely with `transparency_sorting_distance = 0`
- Setting for debugging purposes: `show_block_bounds_radius_near` (_Desour_)
- Windows touch controls are no longer enabled because they exist
- Setting `smooth_scrolling` to disable smooth scrolling (_grorp_)
- Touchscreens/Android: Button bar replaced with a grid menu for easier access (_grorp_)
- (IME candidate list in Windows (_y5nw_)) - inactive: requires a USE_SDL=1 build.

### World / Server / Environment

- More fine-grained control over anticheat by settings (_zmv7_)
- "Access denied" strings may now be translated (_Emojigit_)
- Setting `server_announce_send_players` to hide the player names from the public server list (_Emojigit_)

### Script API / Modding

- Gettext and plural support for client-side translations (_Ekdohibs_. _y5nw_)
- Option to calculate the scroll container size automatically (_SmallJoker_)
- Player controls API now supports joystick inputs (x/y) (_grorp_)
- 6 textures support for `drawtype = "allfaces*"` nodes (_DragonWrangler1_)
- ABM `without_neighbors` field (_sfence_)
- Death formspec can now be customized (_grorp_)
- Bulk LBM support (_sfan5_)
- Players can now be requested to rejoin on kick (by mods) (_Emojigit_)
- Static glTF models and animated models are now supported (_JosiahWI_, _appgurueu')_
- The hotbar is now mod-customizable (_cx384_)
- `core.get_server_status` and `core.privs_to_string` are now sorted (_zmv7_)
- Overridable HP and breath logic, see `ObjectRef:get_flags` (_sfence_)
- Fix animations not being restartable (requires up-to-date client and server) (_appgurueu_)
- `core.close_formspec` (and equivalent) no longer throw incorrect deprecation warnings (_appgurueu_)
- Object observers API (_appgurueu_)
  - This allows mods to selectively show entities to certain players.
- New functions:
  - `core.ipc_(cas|poll)` for async communication (_sfan5_)
  - `core.bulk_swap_node` (_ryvnf_)
  - `core.colorspec_to_table`, `core.time_to_day_night_ratio` (_grorp_)
  - vector utils (`ceil`, `sign`, `abs`, `random_in_area`) (_kromka-chleba_)
  - `core.is_valid_player_name` (_appgurueu_)
  - `vector.random_direction` (_kno10_)
  - `table.keyof` (_Emojigit_)
- Fixes related to ObjectRef lifecycles and crashes caused by attachments (_sfan5_)
- Various script API documentation improvements (_Zughy_, _nauta-turbidus_, _appgurueu_)

### Misc / Maintenance

- Lots of renaming work (_grorp_, _appgurueu_, _Wuzzy_)
- Server + client build targets will now build faster (_sfan5_)
- Tracy profiler support (_Desour_)
- Many cleanups and performance improvements (_sfan5_)
- Network improvements (_sfan5_, _red-001_)
- Compiling/build improvements and fixes (_nerzhul_, _AMDmi3_, _Zughy_, _sfan5_, _SmallJoker_)
- devtest game improvements (_appgurueu_, _Desour_)
- Various crash/sanity fixes

## 5.9.0 → 5.9.1

Released on 15 September 2024.

### Highlights

- Many bugfixes :)

### Client / Audiovisuals

- Reverted rendering optimizations that caused anomalies with certain hardware (_grorp_)
- Main menu: Mapgen flags starting with "no\*" are no longer shown (duplicate) (_appgurueu_)
- Android: Fixed input issues (_grorp_)
- Android: The minimap can again be proportional to the screen size (requires an up-to-date server) (_grorp_)
- Sneak-sumping onto a stack of 2 nodes is again possible (_SmallJoker_)
- Clouds no longer disappear when approaching them (_sfan5_)
- The CSM help dialogue now uses "." instead of "/" (_zmv7_)
- Windows: disabled touchscreen auto-detection due to lack of support of the non-SDL build (default for 5.9.x) (_rubenwardy_)

### World / Server / Environment

- Network improvements for slow connections (_sfan5_)

### Script API / Modding

- yawsprite entities now work on OpenGL ES 2 (Android) (_sfan5_)
- Fixed animation not playing back on upright sprite entities on most systems (_appgurueu_)
- Bone overrides again return the same angles that were specified by mods to ease motion interpolation (_appgurueu_)
- Fixed animations not being restartable (_appgurueu_)
- `minetest.show_formspec` with an empty formspec name is no longer marked as deprecated (_appgurueu_)

### Misc / Maintenance

- macOS 12 support (_sfence_)
- Various fixes
- Build fixes (_AMDmi3_)
- Windows: removed symlink that caused issues while extracting the game archive (_sfan5_)

## 5.8.0 → 5.9.0

Released 2024-08-11

### Highlights

- Rendering performance improvements (_paradust7_, _sfan5_ & _x2048_)
- Added godrays shader (_x2048_)
- New multithreaded Lua mapgen API to improve performance of custom mapgens. (_sfan5_)
- Android: Punch with short tap (_grorp_)
- Work in the background on switching to SDL2 for windowing and input (but not enabled in this release)

### Deprecations and compatibility notes

- `mod_translation_updater.py` is now located at [https://github.com/luanti-org/modtools](https://github.com/luanti-org/modtools) (_Zughy_)
- The setting `opaque_water` is now called `translucent_liquids`. (_Xeno333_)
- Disabling fog and camera updates now requires the "debug" privilege (_sfan5_)
- Since 5.9.0-dev, the Minetest repository now includes IrrlichtMt. The minetest/irrlicht repository is no longer a dependency of newer releases.
- nodebox and mesh nodes now require `use_texture_alpha` to render with transparency (_sfan5_)
- In the HUD elements definition, `hud_elem_type` was renamed to `type` (_cx384_)
- Trusted mod directories are now readonly. Writing to any mod directory is now deprecated (_rubenwardy_)

### Work on SDL2

As part of Minetest's efforts to modernise and improve graphics code, we've been working on switching to SDL2 for windowing and input. This will allow all platforms to support touch screen, keyboard+mouse, and gamepads. Due to remaining issues, Minetest 5.9.0 does not use SDL2 yet. But the ground work is there to support it in the future

**Note: none of the following is available in 5.9.0 as SDL2 is disabled**

- Improved cross-platform touchscreen support. You can now use touchscreen controls on desktop without a special build. (_grorp_, _okias_)
- F11 to toggle fullscreen
- Support for hi-def displays

### Client / Audiovisuals

- New setting `contentdb_enable_updates_indicator` to disable ContentDB requests (_rubenwardy_)
- Make main menu more responsive by using async HTTP calls everywhere (_grorp_)
- Formspecs are now closed by a single tap/click outside (_grorp_)
- Smooth scrolling (_grorp_)
- Add `repeat_dig_time` setting (_ryvnf_)
- Add support for translating content titles and descriptions (_rubenwardy_)
- Rendering performance improvements (_paradust7_, _sfan5_ & _x2048_)
- Add help formspec for CSM commands (_Zemtzov7_)
- Android: Fix app name in the notifications (_srifqi_)
- Android: Add selection dialog (drop down/combo box) (_srifqi_)
- Remove controls listed in the pause menu (not suited for touch inputs) (_Zughy_)
- Remove server's address and port from pause menu (_srifqi_)
- Fix semi-transparent appearance of objects (players/entities) when shaders are disabled (_SmallJoker_)
- Allow mods to swap the meaning of short and long taps (punch with short tap) (_grorp_)
- Recognize double-taps as double-clicks (_grorp_)
- Make the loading screen progress bar respect `gui_scaling` (_grorp_)
- Initial implementation of 'Godrays' shader (_x2048_)
  - Make volumetric light effect strength server controllable (_lhofhansl_)
- Android: Pause rendering while the app is paused (efficiency) (_grorp_)
- Add dithering, enabled by default, setting `debanding` (_HybridDog_)
- `sound_volume_unfocused` for custom sound volume on focus loss (_srifqi')_

### World / Server / Environment

- Minimap textures can now be overwritten by the server (_cx384_)
- Add flag to control mgv6 temple generation (_sfan5_)
- Android: safer and more reliable setting saving (_grorp_)
- Fix multiple password changes in one session (_savilli_)
- More performant improvement object (player/entity) management (_sfan5_)
- Mapblock loading / view range fixes (_lhofhansl_)
- Setting `protocol_version_min` (_Warr1024_)
- Inventory changes
  - Usability fixes (_sfence_)
  - Reordering of inventory callbacks (_SmallJoker_)
  - Prevent item loss when stacking oversized ItemStacks (_SmallJoker_)
- Avoid movement jitter while attached (_lhofhansl_)
- Fixed improper liquid flow (_ZenonSeth_)

### Script API / Modding

- `core.override_item` additional parameter for fields to remove (_appgurueu_)
- Collision moveresult now provide the object's own position (_grorp_)
- Add physics overrides for walk speed and Fast Mode (_grorp_)
- Allow `nil` puncher in `` ObjectRef:punch(..)` `` (_sfence_)
- HUD: Text element color support (_SmallJoker_)
- Allow optional actor in `core.(place|dig|punch)_node` (_Emojigit_)
- Add `button_url[]` and hypertext element to allow mods to open web pages (_rubenwardy_)
- Add world-independent storage directory for mods (_rubenwardy_)
- ItemMetaData-controlled pointing range (_cx384_)
- Add L-system trees as decorations (_cx384_)
- Expose SHA256 algorithm to Lua (_sfan5_)
- Multi-threaded Lua for mapgen (_sfan5_)
  - This is a set of new API functions
- Allow `dynamic_add_media` at mod load time (_sfan5_)
- Move hard coded minimap to Lua (builtin) (_cx384_)
- Tool items: Add wear bar color API (_techno-sam_)
- Improved `[combine` parameter checks (_sfan5_)
- Fog API
  - Fix fog moon tint not working (_appgurueu_)
  - Allow fog color to be overridden in all cases (_sfan5_)
- Fix revoke callbacks being run for `false` values passed to `set_privileges` (_appgurueu_)
- Implement `pointabilities` API to selectively point or block the raycast of tool items (_cx384_, _lhofhansl_)
  - \+ Lua raycast support (_grorp_)
- Add rotation support for wallmounted nodes in 'ceiling' or 'floor' mode (_Wuzzy_)
- Add API for restoring PseudoRandom (discouraged) and PcgRandom (recommended) state (_sfence_)
- Add `player:hud_get_all()` (_cx384_)
- Fix dividing by zero crashes in texture modifiers (_cx384_, _SmallJoker_)
- More modder-friendly output of the Lua profiler (_fluxionary_)
- Add `ObjectRef:add_pos(..)` (_sfence_)
- Rename `hud_elem_type` to `type` (_cx384_)
- Fix `on_(grant|revoke)` not being run by mods (_appgurueu_)
- Extend bone override capabilities (`ObjectRef:set_bone_override`) (_appgurueu_)
- Add `touch_controls` to `core.get_player_window_information()` (_grorp_)
- Persistent formspecs (re-sent on close) can no longer be closed (_SmallJoker_)

### Misc / Maintenance

- Many different documentation improvements
- Mod translation script improvements (_srifqi_, _sfan5_)
- Rename `MINETEST_SUBGAME_PATH` to `MINETEST_GAME_PATH` (_cx384_)
- Updates of the bundled libraries and Lua fixes (via _sfan5_)
- Support OpenGL 3 (_numberZero_)
- Network code maintenance (_sfan5_)
- Build system maintenance (_sfan5_, _siboehm_)
- Unittest and benchmark improvements (_sfan5_, _numberZero_)
- Other code maintenance and improvements (_sfan5_)
- Update to Android SDK 34 (_rubenwardy_)

### Minetest Game

- No longer bundled in Minetest. Please have look at [https://github.com/luanti-org/minetest_game/](https://github.com/luanti-org/minetest_game/)

## 5.7.0 → 5.8.0

Released on 4 December 2023.

### Deprecations and compatibility notes

- **Minetest Game is no longer the default game and will no longer be shipped by Minetest.** If you want it back, install it by using the “Content” tab
- `lua_api.txt` has been converted to Markdown and renamed to `lua_api.md`
- Android now builds via CMake (_sfan5_)
- Compiling: C++17 support is now required
- Node definition field `air_equivalent` is now documented—as deprecated.
- Reading/defining initial object properties directly from an entity definition is deprecated; they should be moved to `initial_properties`

### Client / Audiovisuals

- Main menu: Redesign and unify settings interface (_rubenwardy_, _grorp_, icon by _Zughy_)
- Main menu: better prompt to install a game when none is installed (_ROllerozxa_)
- Main menu: various fixes (_grorp_, _ROllerozxa_)
- ContentDB GUI: Load package list asynchronously (_grorp_)
- Option to invert direction or disable mouse wheel for hotbar item selection (_srifqi_)
- Inventory mouse shortcut improvements (_OgelGames_)
  - Holding down Shift+click while moving the mouse over item slots now continuously moves items to other inventory (if available)
  - Press Shift+click on the crafting output slot to craft and move result to inventory
    - Left mouse button: Craft as many as possible
    - Mouse wheel: Craft 10 times
    - Right mouse button: Craft once
  - Drag an item stack over empty slots to split stacks evenly
  - Hold down Left mouse button while holding an item stack and move the cursor over the slots to pick up items of the same type
  - Double-click an item stack to pick up all items of the same type in this inventory
- Implement `check_offset` for decorations (_nephele-gh_)
- Touchscreen input improvements (_srifqi_)
- Rendering-related performance improvements and fixes (_numberZero_)
- Add antialiasing filters (FXAA, SSAA) (_x2048_)
- Reverse eye-offset Z-coordinate in 3rd person front view (_lhofhansl_)
- DPI-aware crosshair (_grorp_)
- Prevent early respawns caused by up/down button in the death screen (_srifqi_)
- Sounds and animations are now paused in pause menu in singleplayer (_DS_)
- Android: Place nodes with single tap (_grorp_)
- Android: Higher default graphics settings (_grorp_)
- Android: Auto-detect locale (_grorp_)
- Android: ignore broken language files (_srifqi_)
- X11 (Linux): Add primary selection (copy & paste via select & middle-click) support (_DS_)

### World / Server / Environment

- Major speedup for crafting shapeless craft recipes (Hocroft-Karp algorithm) (_DS_)
- Fix crash on handling wallmounted nodes with invalid param2 (_savilli_)
- Fix biomes not respecting their Y limits (_Radar6255_)
  - Especially thin biomes will now be generated as intended.
- Saner (HTTP) timeout limits and log messages (_sfan5_)

### Script API / Modding

- Reading/defining initial object properties directly from an entity definition is deprecated; they should be moved to `initial_properties`
- Add ability to override item images using ItemMetaData (_rubenwardy_)
- Add node pos to node damage HP change reason (_Radar6255_)
- Add `vector.in_area()` utility function (_AFCMS_)
- Add focused styling to buttons (_rubenwardy_)
- Add min/max protocol version to `minetest.get_version()` (_BuckarooBanzay_)
- Add additional texture modifiers (_Treer_)
- Add node group `disable_descend` to disable actively descending down climbable nodes and nodes with liquid move physics (_Wuzzy_)
- Add `VoxelArea::intersect()` (_sfan5_)
- Allow nodes to have their `post_effect_color` affected by lighting (_grorp_)
- Fix potential freeze in `core.check_for_falling` (_savilli_)
- Send everlasting particle spawners to all players (_chmodsayshello_)
- Allow `place_param2 = 0` node placement predictions (_SmallJoker_)
- New player physics overrides for climb speed, sneak speed, acceleration, liquid fluidity and liquid sink speed (#11465) (_Wuzzy_)
- Allow to set custom third person front view camera offset (_grorp_)
- Add script to update/generate mod translations: `util/mod_translation_updater.py` (_Wuzzy_)
  - See `util/README_mod_translation_updater.md` for details
- Add `start_time` to sound parameter tables (part of #12764) (_DS_)

### Misc / Maintenance

- Entity/Object fixes and unit tests (_numberZero_)
- Lua environment cleanups and improvements (_sfan5_)
- Various documentation improvements (_Zughy_, _Wuzzy_)
- Inventory code fixes (_SmallJoker_, _DS_)
- Many various code fixes (_sfan5_, _grorp_, _srifqi_)
- Opt-out option for Doxygen generation on build (_nerzhul_)
- Sound code cleanups and improvements (#12764) (_DS_)
  - Long sounds in sound packs, or sent via dynamic media, no longer cause client freezes on load
  - Positional sounds can be faded now
  - Documentation
  - Other improvements listed elsewhere
- Faster client load times (#12764 and irr#233) (_DS_)

### Minetest Game

- **Minetest Game is no longer the default game and no longer installed by default**
- New water textures (the old ones were [non-free](https://github.com/luanti-org/minetest_game/issues/3051)) (_Lopano_)
- Improve leaves textures in "Opaque Leaves" mode (_Wuzzy_)
- When a player dies in protected air, bones now spawn as a block instead of dropping as an item (_OgelGames_)
- Add API for sapling growth (_aegroto_)
- Hook callbacks for `default.set_inventory_action_loggers` (_appgurueu_)
- Fix logic error in bed rotation (_fluxionary_)
- Fix coral and kelp duplication glitch with sticky piston from Mesecons mod (_zmv7_)
- Fix players being able to skip many nights at once by spam-clicking bed (_appgurueu_)
- Fix not updating vessel shelf infotext (_Niklp_)
- Fix bookshelf infotext not updating when adding, removing or moving items inside (_Montandalar_, _appgurueu_)
- Update translations: German (_Wuzzy_), Spanish (_David Leal_), Ukrainian (_Andriy_), French (_xin_)

## 5.6.0 → 5.7.0

Released on 8 April 2023

### Deprecations and compatibility notes

- The default key for pitchmove was removed. Specify a key manually to use this feature.
  - See [https://github.com/luanti-org/luanti/pull/13319](https://github.com/luanti-org/luanti/pull/13319) for details
- Special handling of `${key}` syntax in metadata values are deprecated
  - See [https://github.com/luanti-org/luanti/pull/12970](https://github.com/luanti-org/luanti/pull/12970) for details
- Worlds with unresolved dependencies can no longer be loaded. This ensures that the specified mods are loaded properly.
  - See [https://github.com/luanti-org/luanti/pull/12542](https://github.com/luanti-org/luanti/pull/12542) for details
- The default key for (un)limited range was removed. Specify a key manually to use this feature.
  - See [https://github.com/luanti-org/luanti/pull/12632](https://github.com/luanti-org/luanti/pull/12632) for details
- Development Test is no longer being distributed in official Minetest releases
  - This was never meant for players to begin with, this “game” is exclusively meant for engine development
  - To get it back, build Minetest from source code (recommended) or download Development Test from [ContentDB](https://content.luanti.org/packages/Minetest/devtest/)

### Client / Audiovisuals

- Fix main menu error when submitting invalid port numbers (_GoodClover_)
- Fix ChatPrompt crash in very narrow windows (_DS_)
- Fix missing shadows when sun tilt is zero (_x2048_)
- Android: Make OpenGLES 2 the default driver (_ROllerozxa_)
- 8x block meshes for improved performance (_x2048_)
  - Configuration options and bugfixes (_lhofhansl_, _x2048_)
- Decrease minimum for `repeat_place_time` (_DS_)
- Fix Enter key after creating a new world (_srifqi_)
- Improve chat history (_TurkeyMcMac_)
- Add dynamic exposure correction (_x2048_)
  - This is also configurable by the Lua API
- Improve the occlusion culling algorithm (i.e. better efficiency) (_x2048_)
- Use multiple threads for mesh generation (i.e. faster rendering) (_x2048_)
- Removed pageflip 3D mode (because broken) (_ROllerozxa_)
- Fix progress bar look on HiDPI displays (_kilbith_)
- Fix `plantlike_rooted` world-aligned node base textures (_TurkeyMcMac_)
- Fix issues caused by attached node placement prediction (_TurkeyMcMac_)
- Avoid shadow flicker at certain angles (_x2048_)
- Chat: fix the unicode characters crowded together on prompt (_snowyu_)
- Take geographic distance into account for server list ordering (_sfan5_)
- Fix sneaking on nodes with large collision boxes (_SmallJoker_)
- Faster light calculations for rendering (_TurkeyMcMac_)
- Android: Improve double-tap for jump detection (_srifqi_)
- Add Bloom shader (_x2048_)
- Restore and enhance bouncy behavior (_pecksin_)
  - Bouncy nodes now let you control the jump height with the jump/sneak keys
- Fix `liquid` drawtype faces sometimes not rendering (_Wuzzy_)
- Apply DPI Scaling to the main menu (_ElliottLester_)
- Improve shadow updates efficiency (_x2048_)
- Textures: introduce world-align overrides (_SmallJoker_)
- Fix crash when stars are reset (_Zughy_)

### World / Server / Environment

- Reduce server CPU consumed by occlusion culling (_lhofhansl_)
- Improve loaded block handling (i.e. better efficiency) (_lhofhansl_)
- Fix `/help` privs checks (_TurkeyMcMac_)
- Add mod storage PostgreSQL backend (_TurkeyMcMac_)
- Update floating nodes when liquid underneath vanishes (_TurkeyMcMac_)
- Add zstd compression support to API function (_20kdc_)

### Script API / Modding

- Server: Fix error caused by sending too long chat messages (_SmallJoker_)
- Correct handling of leftover items in `core.item_eat` (_DS_)
- Various `lua_api.txt` clarifications and fixes (_Wuzzy_, _jordan4ibanez_, _kab0u_, _veprogames_, _aerkiaga_, _DS_)
- Improve `minetest.close_formspec` server-side safety (_luk3yx_)
- Handle nodes changed within another LBM and ABM loop (_TurkeyMcMac_)
- Fix segfault caused by invalid PNG data in `[png`: (_SmallJoker_)
- Add `minetest.get_player_window_information()` (_rubenwardy_, _DS_ (bugfix))
- Make `body_orbit_tilt` configurable (_sofar_)
- Add chat HUD flag (#13189) (_GreenXenith_)
- Improve `MetaDataRef:{get,set}_float` precision (_TurkeyMcMac_)
- Fix error caused by an empty separator for `string.split` (_TurkeyMcMac_)
- Add `player:set_lighting( {saturation = float} )` (_lhofhansl_)
- Add callback `on_mapblocks_changed` (_TurkeyMcMac_)
- Improved Lua error handling (_TurkeyMcMac_)
- Expose `dtime_s` to LBM handler (_sfan5_)
- Let mods choose a forceload limit (_TurkeyMcMac_)
- Add `minetest.get_mapgen_edges` (_TurkeyMcMac_)
- Add `minetest.get_game_info` and allow reading game.conf (_TurkeyMcMac_)
- Add support for facedir/4dir nodes to be attached with `attached_node` (_Wuzzy_)
- Add additional `attached_node` options: always attach to ceiling, always attach to floor (_Wuzzy_)
- Fix errors caused by schematic reading (_TurkeyMcMac_)
- Fix `set_nametag_attributes` resetting the text in subsequent calls (_snowyu_)
- game.conf: Add setting to use volatile a map backend (_SmallJoker_)
- Allow rotating entity selectionboxes (_appgurueu_)
- Add VoxelArea() constructor for easier use (_TurkeyMcMac_)
- Fix formspec focus issue caused by empty element names (_DS_)
- Faster vector, node and content ID access when using LuaJIT (_TurkeyMcMac_)
- Speed up `find_nodes_in_area` (_TurkeyMcMac_)
- Add an item pick up callback (_DS_)
- Implement tool use sounds (_sfan5_)
- Fix inconsistent craft replacement behavior (_Wuzzy_)
- Fix potential error in craft recipes (_savilli_)
- Add paramtype2s `4dir` and `color4dir` for 4 horizontal rotations and 64 colors (_Wuzzy_)
- Bugfix: Allow looped animation to be used safely with old clients (_sfan5_)
- Reassure previous nil behavior for `tiles` and `special_tiles` (_Zughy_)
- Add buffer argument to `VoxelManip:get_light_data` (_TurkeyMcMac_)
- Fix crash when crafting callbacks return strings (_Zughy_)

### Misc / Maintenance

- Fix crash while exiting to the main menu on macOS (_x2048_)
- Rendering code cleanups (_x2048_)
- Fix occasional black screen on startup (_x2048_)
- Android: Build and logging improvements (_sfan5_)
- Improve installation instructions (_lynx197_, _sofar_, _tamara-schmitz_)
- Various code cleanups and optimizations (_sfan5_, _ROllerozxa_, _nerzhul_, _GermanAizek_)
- Implement --debugger option to improve UX when debugging crashes (_sfan5_)
- Various Development Test changes
  - Many, many additions and improvements (_Wuzzy_)
  - Add jukebox and branding iron (_DS_)
- Development Test is no longer officially distributed with Minetest releases
- Android: various maintenance and fixes (_srifqi_)
- Unittest improvements (_Wuzzy_, _TurkeyMcMac_, _rubenwardy_)

### Minetest Game

- Limit and sanitize formspec fields (_appgurueu_)
- Fix `player_api.set_model` not updating the animation (_appgurueu_)
- Ensure chests close properly (_fluxionary_)
- Ensure proper creative hand override (_AntumDeluge_)
- Fix error if /home is executed with an invalid name (_zmv7_)
- Fix wall craft registrations (_alek13_)
- Screwdriver: 4dir node support (_Wuzzy_)

## 5.6.0 → 5.6.1

Released on 19 September 2022.

### Client / Audiovisuals

- Fix tooltips for dropdown, scrollbar and more (_Desour_)
- Allow the comma as clickable URL component (_pecksin_)
- Correct the entity glow calculation (_x2048_)
- Get the setting `texture_min_size` to work again (_fluxionary_)
- Scale hardcoded/integrated GUIs with the system-reported DPI (_ElliottLester_)
- Overwriting a package via "Content" no longer triggers an error (_rubenwardy_)

### World / Server / Environment

- Fix potential use-after-free with item metadata (_TurkeyMcMac_)
- Compatibility patch to not freeze older clients due to negative `frame_length` Tile Animation values (_sfan5_)
- Dynamic shadows performance improvement by delaying non-urgent mapblocks (_x2048_)

### Script API / Modding

- Fix several crashes caused by `clear_craft` in combination with aliases (_savilli_)
- Serialization: Restore (full) pre-5.6.0 compatibility (_appgurueu_)
- LuaJIT: Workaround to allow larger serializations (_appgurueu_)
- Enforced `hp_max > 0` for entities (_appgurueu_)
- Node Definition `tiles` and `special_tiles` again default to `nil` when not specified (_Zughy_)
- Allow `minetest.register_on_craft` to return strings (was: ItemStack) (_Zughy_)
- `ObjectRef:set_stars` to reset the stars no longer throws an error (_Zughy_)

### Misc / Maintenance

- x86 Android build fixes (_savilli_)

## 5.5.0 → 5.6.0

Released on 4 August 2022

### Deprecations and compatibility notes

- `name` in game.conf is deprecated for the game title
  - For specifying the game title from now on, use `title` instead

### Client / Audiovisuals

- Dynamic shader-based shadows for: nodes, entities, wield (_x2048_)
  - Includes many, many bugfixes and improvements (tuning, performance)
- Fixed statbar HUD background scaling and numbering (_appgurueu_)
- Apply texture pack main menu textures immediately (_ROllerozxa_)
- Fix footsteps for players whose collision box min y != 0 (_grorp_)
- Add depth sorting for node faces (_x2048_)
  - This fixes appearance issues when looking through multiple semi-transparent nodes.
  - This works only up to a distance of 16 nodes by default. Use the `transparency_sorting_distance` setting to adjust this
- Optimize swapping nodes with equivalent lighting (_TurkeyMcMac_)
- Fix item entity Z-fighting (_appgurueu_)
- Use mod names/titles instead of technical names to display (_GoodClover_)
- Fix texture packs not showing as enabled in mainmenu (_rubenwardy_)
- Debug screen now shows `<unknown node>` at the top if an unknown node is pointed (_Wuzzy_)
- Enable chat clickable weblinks by default (Ctrl+Click) (_Froggo_)
- HUD: Fix outdated selection boxes (_appgurueu_)
- Make `no_screenshot` image more clear (_Zughy_)
- Add register dialog to separate login/register (_rubenwardy_)
- No damage effects on `hp_max` change (_appgurueu_)
- Fix updating glow and light calculation on entities (_sfan5_)
- Fix unknown nodes sometimes displaying the "no texture" instead of the "unknown node" texture (_Wuzzy_)

### World / Server / Environment

- Distinct mod path values in world.mt to avoid issues with duplicated mod names (_rubenwardy_)
- Fix broken server startup if curl is disabled (_sfan5_)
- Increase max. objects per block defaults (_appgurueu_)
- Builtin: Allow to revoke unknown privileges (_SmallJoker_)
- Fix some textures not being sent correctly to older clients (_Oblomov_)
- Fix several registration/authentication related issues (_sfan5_)
- Fix dependency enabling of mods and modpacks (_rubenwardy_, _TurkeyMcMac_)
- Fix cooking and fuel crafts with aliases (_TurkeyMcMac_)
- Commands: Some numbers can be replaced or prepended with `~` for values relative to the current one (_Wuzzy_)
  - `~` is equivalent to `~0`
  - Supported commands: `/deleteblocks`, `/emergeblocks`, `/fixlight`, `/spawnentity`, `/teleport`, `/time`
  - Example: `/teleport 15 ~5 ~` teleports to (15, `<current Y coordinate plus 5>`, `<current Z coordinate>`)
- Don't allow banning in singleplayer (_sfan5_)
- Docs: Add description of privileges (_x2048_)
- Increase max FPS on Android to 60 (_ROllerozxa_)
- Add many limits to settingtypes + engine (_Wuzzy_, _SmallJoker_)
- Reorganize settingtypes.txt (_rubenwardy_)

### Script API / Modding

- Improved formspec documentation (_DS_)
- Optimization: Send HUD flags only if they changed (_appgurueu_)
- Allow to set the displayed item count and its alignment via item meta: `count_meta`, `count_alignment` (_DS_)
- Add support for 'seed' in `disallowed_mapgen_settings` (_Wuzzy_)
- List of documentation improvements:
  - `AreaStore` (_SmallJoker_)
  - Lua `vector` helper class (_sfan5_)
  - `spawn_by` for decorations (_Zughy_)
  - LBM documentation (_TurkeyMcMac_)
  - Overall improvements (_sfan5_)
- Allow `get_sky` to return a table of all sky-related parameters (_Zughy_)
- Add `basic_debug` HUD flag to control display of debug info like position in the debug screen (on by default) (_appgurueu_)
- Fix memory leak from `SpatialAreaStore` (_setupminimal_)
- Add function `ObjectRef:set_lighting()` to control shadow intensity from the game/mod (_x2048_)
- Fix `[combine` when `EVDF_TEXTURE_NPOT` is disabled (_paradust7_)
- `hud_get`: Return precision field for waypoint (_appgurueu_)
- Add Async environment for parallelized Lua code execution (_sfan5_)
  - `minetest.handle_async`
  - `minetest.register_async_dofile`
- Fix Minetest blaming the wrong mod for errors (_appgurueu_)
- Deprecate game.conf `name`, use `title` instead (#12030) (_rubenwardy_)
- Protect a few more settings from being set from mods (_sfan5_)
- Add function `ObjectRef:respawn()` to invoke player respawn (_sfan5_)
- Handle Lua entity HP changes correctly (like punches) (_sfan5_)
- Add tool helper function `ItemStack:add_wear_by_uses()` to add tool wear in such a way that it has a given number of uses (_Wuzzy_)
- Add `minetest.get_tool_wear_after_use` to simulate tool wear when expecting it to break after a given number of uses (_Wuzzy_)
- `on_deactivate` entity callback: distinguish removal and unloading (_appgurueu_)
- Remove `tile_images` and `special_materials` obsolete code (_Zughy_)
- `set_stars`: Allow to set maximum star opacity at daytime with `day_opacity` (_Wuzzy_)
- FormSpec: 9-slice images, `animated_image`, and `fgimg_middle` (_v-rob_)
- Animated particle spawners (_velartrill_)

### Misc / Maintenance

Code details are intentionally omitted due to the changelog target audience's interests.

- Fix macOS compile instructions (_sfan5_)
- Various C++ code cleanups and improvements (_TurkeyMcMac_, _sfan5_, _Oblomov_, _SmallJoker_, _Octavian_, _RichardTry_, _savilli_, _JosiahWI_)
- List of DevTest game improvements:
  - TGA test nodes (_erle_)
  - Test weapons and armorball modes (_Wuzzy_)
  - Nodes and items for testing overlays (_Wuzzy_)
  - Entity lifecycle and callbacks (_sfan5_)
  - Item metadata editor (_Wuzzy_)
- Minetest now uses C++14
- Remove direct OpenGL(ES) dependency (_sfan5_)
- Compile Lua as C++ to properly catch exceptions (_TurkeyMcMac_)
- Build system improvements (_sfan5_, _ShadowNinja_, _LoneWolfHT_)
- Run automated tests when Lua files change (_x2048_)
- Add JSON (de)serialization benchmarks (_paradust7_)
- Performance optimizations by caching (mapblocks, collisionbox) (_sfan5_)
- Add more Prometheus metrics (_sfan5_)
- Add documentation to list breaking changes for the next major release (_Zughy_)
- Patch built-in Lua to fix miscompile on Android (_paradust7_)
- Fix BSD iconv declaration (_savilli_)
- Fix Android input box crash (_ROllerozxa_)

### Minetest Game

- Improved cart movement behavior (_SmallJoker_)
  - Improved direction handling
  - Smoother-out 'end-of-rail' animation
  - Other improvements
- Dynamic shadow intensity increases with cloud density (only has an effect if you have dynamic shadows enabled) (_lhofhansl_)
- Allow mods to override player animation globalstep with `player_api.globalstep` (_LoneWolfHT_)
- Log API added (_nixnoxus_)
- Fix crash if player has no model (_Lars Mueller_)
- Fix broken `get_animation` in `player_api` (_bell07_)
- Fix furnace fire sound continuing to play after being destroyed (_Wuzzy_)
- Fix TNT blowing up `ignore` nodes (_Wuzzy_)
- Fix some hoes not breaking after the intended number of uses (_Wuzzy_)
- Fix book duplication glitch (_appgurueu_)
- Fix incorrect behavior of glass and obsidian glass if param2 was changed (_appgurueu_)
- Fix cart sometimes facing the wrong way at slopes (not a 100% perfect bugfix tho) (_SmallJoker_)
- New translation: Polish (_mrubax10_)
- Translation updates: Ukrainian (_baytuch_), Russian (_baytuch_), German (_Wuzzy_), Lojban (_Wuzzy_), Esperanto (_quarthex_)

## 5.5.0 → 5.5.1

Released on 15 May 2022.

### World / Server / Environment

- Fix server crash due to duplicate user registrations (_sfan5_)
- Fix cooking and fuel crafts with aliases (_Jude Melton-Houghton_)
- Fix some textures not being sent correctly to older clients (_Giuseppe Bilotta_)
- Fix broken server startup if curl is disabled (_sfan5_)
- Fix password changing getting stuck if wrong password is entered once (_sfan5_)
- Apply `disallow_empty_password` to password changes too (_sfan5_)

### Client / Graphics

- Fix various issues with Select Mods and Content (_rubenwardy_, _Jude Melton-Houghton_, _Alex_)
- ContentDB: Fix ungraceful crash on aliases when list download fails (_rubenwardy_)
- Fix performance issue due to hardware buffer counters (_paradust7_)
- HUD: Update selection highlight every frame to avoid glitches (_Lars Müller_)
- Fix `[combine` when `EVDF_TEXTURE_NPOT` is disabled (_paradust7_)
- Fix footsteps for players whose collision box min y != 0 (_Gregor Parzefall_)
- Fix undefined behavior in TileLayer (_Daroc Alden_)
- Use absolute value for bouncy in collision (_pecksin_)
- Fix builtin statbar backgrounds (_Lars Mueller_)

### Misc

- Fix possible unreliable behavior due to uninitialized variables (_Octavian_)
- Fix Minetest blaming the wrong mod for errors (_Lars Müller_)
- Fix some memory leaks (_SmallJoker_, _Daroc Alden_, _Daroc Alden_)

### Minetest Game

- `player_api` mod: Fix crash if player has no model (_appgurueu_)
- `player_api` mod: Mods can now override globalstep by overriding `player_api.globalstep` (_LoneWolfHT_)
- Shadow intensity (of dynamic shadows) changes with weather (_lhofhansl_)
- Some cart movement behavior fixes (_SmallJoker_)
- Fix some translations in uk and ru locales (_baytuch_)

## 5.4.0 → 5.5.0

Released on 30 Jan 2022.

### Deprecations and compatibility notes

- FORMSPEC_API_VERSION is now 5
- New maps are now zstd compressed to reach faster and/or more efficient compression
- Switched to our own fork of the rendering engine: IrrlichtMT
  - Removed support for DirectX
  - Dropped support for obscure and undocumented file formats: pcx, ppm, psd, wal, and rgb
- Modding: Missing "mod.conf" is now deprecated. Results in warnings (_rubenwardy_)
  - Add mod.conf with name = yourmodname
- Modding: depends.txt and description.txt are now deprecated
  - Specify dependencies using "depends" and "optional_depends" in mod.conf
  - Specify description using "description" in mod.conf
- Modding: Creating vectors like this: `{x=1, y=2, z=3}` is now deprecated
  - Use `vector.new` instead
- Bitmap fonts are no longer supported
  - Use TTF fonts instead

### Features: General

- Add game name to server status string (_sfan5_)
- Improve TTF support for pixel-style fonts (_v-rob_)
- Joystick support for DragonRise GameCube controller (_Izzette_)
- Add `MINETEST_MOD_PATH` environment variable (_emixa-d_)
- Touch UI support for desktop builds (#10729) (_TheBrokenRail_)
- Switch MapBlock compression to zstd (_lhofhansl_)
- Joystick sensitivity for player movement (_NeroBurner_) + fixes (_sfan5_)
- Gettext support on Android (_Pevernow_)
- Make web links in chat clickable (Feature disabled by default, use setting `clickable_chat_weblinks`) (_pecksin_)
- Add a key to toggle display map block boundaries (F8 by default) (_grapereader_)
- Improved wording of various chat command outputs (_Wuzzy_)
- Normal texture support (for minimap shading) (again) (_numberZero_)
- Scale mouse/joystick sensitivity depending on FOV (_Elias Åström_)
- Various DevTest game additions and improvements (_Wuzzy_)
- Chat commands: Show the execution time if the command takes a long time (_HybridDog_)
- Improved item placement prediction (_sfan5_)
- Anticheat: Faraway inventory access protection (_SmallJoker_)
- Pause animations while game is paused (_numberZero_)

### Features: Main menu and ContentDB

- Chop game background in mainmenu (_appgurueu_)
- ContentDB: Add support for package aliases / renaming (_rubenwardy_)
- Improved "Join Game" tab (_sfan5_)
- Builtin function translation (_Wuzzy_, _Zughy_)
- Translation support for the builtin functions (_Wuzzy_, _snowyu_) + updates (_Wuzzy_, see CONTRIBUTING file)
- Handle modpacks containing modpacks properly (_Elias Fleckenstein_)
- Texture pack toggle by double clicking (_Yaman Qalieh_)

### Features: Modding

- Sky API: Reset by empty arguments (_Zughy_)
- Use a database for mod storage (internal) + CSM auto-migration (_TurkeyMcMac_)
- Add padding[] element to formspecs (#11821) (_v-rob_)
- Disable inventory if player's inventory formspec is blank (_ROllerozxa_)
- Add `minetest.disconnect_player` (_Corey Powell_)
- Add Lua bitop library (_Lejo_)
- Allow for game-specific menu music (_ExeVirus_)
- Add minetest.rmdir, minetest.cpdir and minetest.mvdir (_octacian_)
- Add `no_texture.png` as fallback for unspecified textures (_Wuzzy_)
- Add `minetest.get_server_max_lag()` (_Wuzzy_)
- Split node field `liquid_viscosity` into two: `liquid_viscosity` (how fast liquid flows) and `move_resistance` (how much it slows players) (_Wuzzy_)
- Improved `dynamic_add_media` functionality (_sfan5_)
- Add group-based tool filtering for node drops (_Treer_)
- Add `disable_settings` to game.conf to get rid of "Enable Damage"/"Creative Mode"/"Host Server" checkboxes (_Df458_)
- Add a simple PNG image encoder with Lua API + texture modifier `[png` (_hecks_)
- Add bold, italic and monospace font styling for HUD text elements (_sfan5_)
- Add wallmounted support for plantlike and `plantlike_rooted` nodes (_Wuzzy_)
- Add API for mods to hook liquid transformation events (_Warr1024_)
- Add `min_y` and `max_y` checks for Active Block Modifiers (ABM) (_sfence_)
- Add metatables to Lua vectors (_DS_)
- Add `minetest.compare_block_status` function (_SmallJoker_)
- Add `minetest.colorspec_to_colorstring` (_v-rob_)
- Put torch/signlike node on floor if paramtype2=="none" (_Wuzzy_)
- Return ObjectRef from `minetest.spawn_falling_node()` (_benrob0329_)
- Modifiable player fall damage via armor group (_Wuzzy_)
- Add `vector.to_string` and `vector.from_string` (#10323) (_DS_)
- Add math.round and fix vector.round (_v-rob_)
- Deg-rotate support for mesh nodes (_numberZero_) + fixes (_sfan5_, _Wuzzy_)
- `lua_api.txt`: Fix style selector examples (_Df458_)
- Nested Settings are now also contained in `to_table` (_SmallJoker_)

### Bugfixes

- Fix Minetest logo when installed system-wide (_ROllerozxa_)
- Cancel emerge callbacks on shutdown (_TurkeyMcMac_)
- Free arguments of cancelled minetest.after() jobs (_sfan5_)
- Fix damage wraparound if very high damage (_Wuzzy_)
- Cap damage overlay duration to 1 second (_Wuzzy_)
- Rendering fixes: Add more neighbors on mesh update (_numberZero_)
- Don't let HTTP API pass through untrusted function (_sfan5_)
- Fix URL escaping in content store (_sfan5_)
- Fix `find_nodes_in_area` misbehaving with out-of-map coordinates (_sfan5_)
- Minimap: gamma-correct average texture colour calculation (_HybridDog_)
- Fix item duplication if player dies during interact callback (_sfan5_)
- View bobbing fixes (_appgurueu_)
- Fix player HP desync between client and server (_savilli_)
- Rendering fixes: Order drawlist by distance to the camera (_x2048_)
- Fix crash when .conf release field is invalid (_rubenwardy_)
- Performance: Fix client-side performance of chat UI (_DS_)
- Fix HUD multiline text alignment (_appgurueu_)
- Send correct updates to clients after node metadata clear (_TurkeyMcMac_)
- Remove redundant `on_dieplayer` calls (_savilli_)
- Fix 6th line of infotext being cut off in half (_Wuzzy_)
- Validate staticdata and object property length limits (_sfan5_)
- Fix scaled world-aligned textures being aligned inconsistently for non-normal drawtypes (_Wuzzy_)
- Various `lua_api.txt` corrections and improvements (_Df458_, _random-geek_, _Wuzzy_, _Francisco_, _Zughy_)
- Run `on_grant` and `on_revoke` callbacks after privs change (_AFCMS_)
- Fix base64 validation and add unittests (_appgurueu_)
- Fix cloud fog being broken for high clouds (_Wuzzy_)
- Attachments: various bugfixes (_SmallJoker_)
- Rendering engine fixes and cleanups (_nerzhul_)
- Multiple OpenGL ES fixes (_sfan5_)
- Make edit boxes respond to string input (IME) (_yw05_)
- cURL timeout fixes and increased default timeout (_sfan5_)
- Fix wield image of `plantlike_rooted` (_Wuzzy_)
- Fix attached-to-object sounds volume (_Desour_)
- Fix segfault for model[] without animation speed (_kilbith_)
- Crash fix when models fail to load (_sfan5_)
- Access protections for per-player detached inventories (_SmallJoker_)
- `mg_name` and `mg_flags` can no longer be set by Lua (minetest.conf) (_sfan5_)
- Interlaced 3D mode fixes (_srifqi_)
- Fix `hud_change` and `hud_remove` functionality after `hud_add` calls (_savilli_)
- Fix number of times a tool can be used before breaking being off by a number between 1 and 32767 (_Wuzzy_)
- Various stability fixes (server and client crashes)

### Maintenance

- Rendering improvements: use dedicated GPU, improve frame calculations (_sfan5_)
- Fully remove bitmap font support (use TTF now) (_sfan5_)
- Restore GCC 5 compatibility (_JosiahWI_)
- Remove creative/damage info in Esc/Pause menu (_Wuzzy_)
- Update to Android target SDK 30 (_rubenwardy_)
- Add macOS build docs (_andkerr_)
- Android: Use scoped app storage (_rubenwardy_)
- Make /status message easier to read (_Wuzzy_)
- Clean up/improve some scriptapi error handling code (_sfan5_)
- Add hint to error message on how to build with in-tree Irrlicht (_20kdc_)
- Optimize vector length calculations (_Lean Rada_)
- Remove hardcoded "You died." message in chat (_Wuzzy_)
- Remove unsupported video drivers (_hecks_)
- Document hypertext formspec element escaping (_Wuzzy_)
- Drop --videomodes, `fullscreen_bpp` and `high_precision_fpu` settings (_sfan5_)
- PostgreSQL fixes and improved error messages (_sfan5_)
- Improved liquid documentation (_Wuzzy_)
- Improved mipmapping-related code (_sfan5_)
- Rendering engine was changed from Irrlicht to IrrlichtMt (Minetest's fork of Irrlicht) (_sfan5_)
- Performance: Draw items as 2D images (instead of meshes) when possible (_sfan5_)
- Sanity check: Block & report player self-interaction (_appgurueu_)
- Multiple font code cleanups and improvements (_sfan5_)
- IrrlichtMt switch related fixups (_kilbith_. _sfan5_, _nerzhul_)
- Performance improvements during media/mesh loading (_sfan5_)
- Json is now taken from the system by default (_sfan5_)
- Various build bot and setup changes (_sfan5_)
- Restructured "/teleport" command (_HybridDog_)
- Consistent Aux1 key naming (_Wuzzy_)
- Many many internal cleanups and fixes (_sfan5_, others)

### Minetest Game

- Add “Read” and “Write” tabs to book interface when you own the book (_orbea_)
- Allow to write books without text or title (_orbea_)
- Make identical keys stackable (_Luis Royer_)
- Fix creative inventory trash slot not working for player named “trash” (_Montandalar_)
- Fix sunlight propagation for glass stair/slab (_An0n3m0us_)
- Fix glass bottle with firefly not being placable in vessels shelf (_An0n3m0us_)
- Other bugfixes
- Translations: Esperanto (_Jason Cartwright_), Russian (_ptah-alexs_), Japanese (_nogajun_), German (_Wuzzy_), Slovak (_Daretmavi_), French (_Olivier Dragon_), Swedish (_ROllerozxa_), Chinese (_雷哲翰_), Ukrainian (_baytuch_)

## 5.3.0 → 5.4.0

Released on 23 Feb 2021.

### Deprecations and compatibility notes

- Removed support for bumpmapping, generated normal maps, and parallax occlusion (_Lars_, _hecks_)
  - These features had fundamental issues, several bugs, and were broken on some platforms.
- Deprecated node field value: `use_texture_alpha = true/false`
  - Fix: Use `clip`, `blend` or `opaque` (see documentation)
- Deprecated `get_player_velocity` and `add_player_velocity` (_rubenwardy_)
  - Fix: replace with `get_velocity()` and `add_velocity()`
- Deprecated multiply and divide with two vectors (Schur product and quotient) (_DS_)
  - Fix: implement your own version
- By default, the crosshair will now change to an "X" when pointing to objects. If your game had a custom crosshair, this might come as a surprise and break graphical consistency.
  - Fix: Specify object_crosshair.png image
- Added deprecation warning for node field: `alpha` (only limited compatibility provided)
  - This was already deprecated and undocumented.
  - Fix: Replace by `use_texture_alpha`
- Fixed deprecation warning when certain ores types ("sheet", "puff", "blob" and "vein") are missing `noise_params` (_rubenwardy_)
  - These ore types require noise_params. To keep the same behavior, you can use the following values:

```
noise_params = {
    offset  = 0,
    scale   = 1,
    spread  = {x=250, y=250, z=250},
    seed    = 12345,
    octaves = 3,
    persist = 0.6,
    lacunarity = 2,
    flags = "defaults",
}

```

### Features

#### General

- Make 'place' and 'dig' keys freely configurable (only via minetest.conf for now: `keymap_place` and `keymap_dig`) (_ANAND_, _Markus Koch_)
- Freely bindable mouse buttons (only via minetest.conf for now: `KEY_LBUTTON`, `KEY_MBUTTON`, `KEY_RBUTTON`) (_ANAND_, _Markus Koch_)
- Add 'ores' global mapgen flag (_Paramat_)
- Mapgen Flat: Add caverns, disabled by default (_Paramat_)
- Semi-transparent background for nametags (_Zughy_, _rubenwardy_)
- Disable object selectionboxes by default (_LoneWolfHT_)
- Change crosshair when pointing to objects ("X" shape by default) (_LoneWolfHT_)
- Shaders for Android (GLES 2) (_Vitaliy_)
- Load media from subfolders (_DS_)
- Allow configuring block disk and net compression. Change default disk level. (_Lars_)

#### Main menu and ContentDB

- ContentDB: Add dependency resolution, update all, and download queues (_rubenwardy_)
- ContentDB: Add overwrite dialog when content is already installed (_rubenwardy_)
- ContentDB: Use icons for buttons (_Zughy_)
- Add open user data button to main menu (_rubenwardy_)
- Main menu: Add clear button and icon for search input (_Andrey_)
- Improve layout of main menu 'local' tab (_Paramat_)

#### Modding: GUI (Formspecs) / HUD

- Add sound effect style option (_Pierre-Yves Rollo_)
- Add 3d model formspec element (_Jean-Patrick Guerrero_, _SmallJoker_, _Thomas--S_)
- Add minimap and compass HUD elements (_Pierre-Yves Rollo_, _Jean-Patrick Guerrero_)
- Make bgcolor tint button background images (_Hugues Ross_)
- Add gradients and borders to FormSpec boxes (_v-rob_)
- Add font styling options (_v-rob_)
- Add `set_focus[]` to initially focus elements (_v-rob_)
- Make dropdowns optionally return event based on index, not value (_v-rob_)
- Avoid drawing clipped out formspec elements (_EvidenceB_)
- Darken tabheader background color (_Kezi_)
- Add inventory list styling: spacing, slot size, and noclip (_v-rob_)
- Add support for custom object crosshair image: `object_crosshair.png` (_LoneWolfHT_)

#### Modding: Other

- Add support for showing attached objects whilst in first person mode (_Jordach_)
- Add ability to cancel a minetest.after call after it was started (_tenplus1_)
- Add `on_rightclickplayer` callback (_sorcerykid_)
- Add `on_deactivate` callback for luaentities (_hecks_)
- Add `minetest.get_objects_in_area` (_Elias Fleckenstein_)
- Add `ObjectRef:get_children()` (_Zughy_)
- Add a `short_description` to be used by mods (_DS_)
- Add `minetest.get_artificial_light` and `minetest.get_natural_light` (_HybridDog_)
- Add `register_on_chatcommand` to SSM and CSM (_Elijah Duffy_)
- Add vector.offset (_DS_)
- Register missing `get_texture_mod` function (_karamel59_)
- `content_cao`: Support texture animation for `upright_sprite` (_sfan5_)
- Add PUT and DELETE request + specific method value to HTTP API (_Lejo_)
- Nodes are now allowed to have the "liquid" or "flowingliquid" drawtype for non-liquids (liquidtype=none) (_Wuzzy_)
- Play `place_failed` sound if trying to place a node into an occupied space or it was an "attachable" node that failed to attach (_Wuzzy_)
- Implement grouped mode for `find_nodes_in_area` (_sfan5_)
- Clean up `sound_fade` (_hecks_)
- Chat commands: Show help message if func returns false without message (_HybridDog_)
- Node use_texture_alpha field supports now 3 modes "blend", "clip" and "opaque" (deprecated true/false values)

### Other enhancements and maintenance

- Cross-reference the node level manipulation functions (_Oblomov_)
- Update fallback fonts and mark additional locales as broken (_sfan5_)
- Clean up `l_object.cpp` (_Zughy_)
- Devtest: Improve various things (_Paramat_, _HybridDog_, _Wuzzy_)
- Android: Add CI with saving artifacts (_Maksim_)
- Add NetBSD cpu affinity support code (_David CARLIER_)
- Android: drop simple MainMenu (_Maksim_)
- Add support for Haiku OS (_David CARLIER_)

### Bug fixes

#### Security

- Prevent players accessing inventories of other players (_Lars Müller_)
- Inventory: Protect Craft and Drop actions (_SmallJoker_)
- Prevent interacting with items out of the hotbar (_Lejo_)
- Fix inventory swapping not calling all callbacks (_Lars Müller_)
- Patch fast/teleport vulnerability when attached to an entity (_Elias Fleckenstein_)
- Prevent games from setting secure settings (_rubenwardy_)
- Prevent players from being able to modify ItemStack meta (_luk3yx_, _rubenwardy_)

#### Other

- Fix dropped craftitems/tools not using `light_source` values (_LoneWolfHT_)
- Fix when `on_player_hpchange` is called (_SmallJoker_)
- Use JSON for favorites list, fixing many bugs (_rubenwardy_)
- Fix hypertext and textarea elements consuming scroll events (_v-rob_)
- Fix ESC in error dialog from closing Minetest (_Yaman Qalieh_)
- Remove null bytes from `TOCLIENT_BLOCKDATA` (_luk3yx_)
- Load system-wide texture packs too (_Zughy_)
- Fix Android support in bump version script (_rubenwardy_)
- ContentDB: Ignore content not installed from ContentDB (_rubenwardy_)
- Sanitize server IP field in mainmenu (_Zughy_)
- Fix item tooltip background color not working (_Lars Mueller_)
- Display Minetest header when `menu_last_game` value isn't available anymore (_Zughy_)
- Fix `minetest.is_nan` (_Lars Mueller_)
- Fix some minor code issues all over the place (_sfan5_)
- Minor profiler fixes. (_Lars_)
- Fix fallnode rotation of wallmounted nodebox/mesh (_Wuzzy_)
- Make installer create its own Minetest folder (_LoneWolfHT_)
- Implement mapblock camera offset correctly (_hecks_)
- Fix MSAA stripes (_HybridDog_)
- Fix certain connected nodeboxes crashing when falling (_sfan5_)
- Avoid generating the same chunk more than once with multiple emerge threads. (_Lars_)
- Fixed various issues with stars, sky, and clouds (_numzero_)
- Fix falling image of torchlike if paramtype2="none" (_Wuzzy_)
- Fix player sprite visibility in first person (_sfan5_)
- Fix object interaction distance not being checked (_rubenwardy_)
- Block attempts to connect to the client (_red-001_)
- Fix segfault in deprecation logging due to tail call, log by default (_rubenwardy_, _SmallJoker_)
- Player physics: Ensure larger dtime simulation steps (_Lars Müller_)
- Avoid resending near blocks unnecessarily. (_Lars_)
- Fix CSMs on arm64 (_luk3yx_)
- Fix Media... 0% on loading screen (_Maksim_)
- Implement unloading of `static_save=false` objects according to existing docs (_sfan5_)
- Decouple entity minimap markers from nametags replacing with `show_on_minimap` property (_sfan5_)
- Periodically release all mesh HW buffers to avoid an Irrlicht bottleneck. (_Lars_)
- Fix float argument check in `minetest.set_timeofday()` (_Zughy_)
- Avoid drawing invisible blocks on the client. (_Lars_)
- Fix scroll bar overlapping text (again) (_random-geek_)
- Reduce the FPS when the window is unfocused (_HybridDog_)
- Android: replace InputDialogActivity on simple dialog window (_Maksim_)
- Correct erroneous reported max lag with prometheus (_Buckaroo Banzai_)
- Fix horizontal/vertical merging bug of hardware-colored framed glass (_Paramat_)
- Fix chat/infotext overlap if many chat lines (_Wuzzy_)
- Settings: Fix crash on exit (_SmallJoker_)
- Record player existence in dummy database. (_Lars_)
- Darwin platform build fix (_David CARLIER_)
- Scale inventory image for scaled allfaces nodes (_Wuzzy_)
- Fix NetBSD build (_David CARLIER_)
- shaders: Fix transparency on GC7000L (_mntmn_)
- Fix MSVC compiler warnings (_adrido_)
- Fix light overflow of u8 if light is saturated at 255 (_BenjaminRi_)
- Fix missing translation call in hypertext (_Pierre-Yves Rollo_)
- Render nodeboxes with opaque material if possible (_sfan5_)
- Fix precision not working in `hud_change` (_Lars Müller_)
- Fix build for Visual Studio (explicitly cast pointers) (_Seeker_)
- Fix GCC class-memaccess warnings (_Paul Ouellette_)
- Falling: Fix error caused by missing param2 (_SmallJoker_)
- Allow starting local server using --go again (_SmallJoker_)
- `decode_base64`: Allow '=' padding character (_SmallJoker_)
- Sanitize world directory names on create. Keep original name separate (_Hugues Ross_)
- Improve bad/missing default inventory+wield images of node drawtypes, affecting: airlike, signlike, torchlike, raillike, plantlike, `plantlike_rooted`, firelike, flowingliquid (_Wuzzy_)
- Android: Fix ConfirmRegistration and PasswordChange input and scale size (_Maksim_)
- Formspecs: volume and key settings windows can now be closed by double-clicking or double-tapping (_Zughy_)

### Minetest Game

- Add crafting guide (_Paul Ouellette_)
- Added 5 wood variants of Mese Post Light (_An0n3m0us_)
- Add environment sounds for lava and active furnace (_An0n3m0us_)
- Change several block sounds (_An0n3m0us_)
- Fix players sleeping in an occupied bed (_An0n3m0us_, _Wuzzy_)
- Fix 'sleepwalking' in bed (_An0n3m0us_, _Wuzzy_)
- Fix sleeping player flying off the bed when damaged and flying far away from the bed after death (_An0n3m0us_)
- Fix sleeping player being immobilized and bed undiggable after death (_An0n3m0us_)
- Fix furnace infotext not always updating when removing item (_orbea_)
- New translation: Slovak (_Daretmavi_)
- New translation: Brazilian Portuguese (_ronaldo_)
- New translation: Lojban (admittedly not a very good translation) (_robintown_, _Wuzzy_ and others)
- Update existing translations (various people)

## 5.2.0 → 5.3.0

Released on 9 July 2020.

### Modder Update Guide

- Lua API: `minetest.PLAYER_MAX_BREATH_DEFAULT` set to 10 (was 11). This can cause problems with “statbar” mods (like [hudbars]) that rely on the old value, so those likely need updating
- Default in Minetest Game was incorrectly overriding dropped items. Make sure to update any overrides to pass new arguments to on_step to the original function

### Client / Audiovisuals

- Smoother camera movement to fix jump glitches (_paramat_)
- More precise player controls handling (_TheTermos_)
- Fix bone-attached entities (_hecktest_)
- Fix incorrect position data on attach (_hecktest_)
- Fix HUD scaling (mainly for Android) (_MoNTE48_)
- Fix incorrect entity light calculations (_SmallJoker_, _sfan5_)
- Add `chat_log_level` and `chat_font_size` setting (_SmallJoker_)
- New default sky color gradient (_TheTermos_)
- Change default keys for camera/minimap to C/V (_Wuzzy_)
- Fix breath bar scaling (_ANAND_)
- Fix bone position changes breaking animations (_theviper121_)
- No grey screen when C++ errors are thrown by Lua (_pauloue_)
- Android: add OpenGL ES 2 support (_MoNTE48_)
- Reuse `object_shader` for "wielditem" and "item" entities (_dcbrwn_)
- Shaders: Fix OpenGL < 4.3 compatibility (_SmallJoker_)
- Formspec: properly display altered inventory lists (_DS_)
- Android: Various input-related fixes (_MoNTE48_)
- Android: Android Studio support, improve everything (_MoNTE48_)
- Remove sound menu and show proper msgs if sound is off (_Wuzzy_)
- Highlight hovered formspec elements when pressing F5, for testing (_SmallJoker_)
- Implement DPI scaling for Windows (_sfan5_)
- Allow relative directories for `screenshot_path` (_Calinou_)
- Add (optional) tone mapping for entities (_dcbrwn_)
- Add new mapgen options in world creation dialog (_Wuzzy_)
- Add support for overriding item inventory/wield images in texture packs (_Df458_)

### GUI (Formspec)

- Fix wrong image button scaling (_pyrollo_)
- Document `*_hovered` and `*_pressed styles` as deprecated (_v-rob_)
- Add buttons to ContentDB in game bar and configure world (_rubenwardy_)
- Add universal style selector “`*`” (_v-rob_)
- Add `content_offset` and `padding` style properties for buttons (_Df458_)
- Add `scroll_container` element (_DS_)
- CSS-like state-selection for style elements (_Df458_)

### World / Server / Environment

- Fix liquids refusing to flow in X+ or Z+ in some cases (_sfan5_)
- Rework functionality of leveled nodes (_Wuzzy_)
- New Mapgen V7 floatland implementation (_paramat_)
- Server: Better emerge multithreading, various fixes (_sfan5_)
- Correctly indicate failure in /spawnentity (_sfan5_)
- Add PostgreSQL authentication backend (_nerzhul_)
- Add chat command "/revokeme (priv)" (_Panquesito7_)
- Optimize `get_objects_inside_radius` calls (_nerzhul_)
- Fix world migration to PostgreSQL (_sfan5_)
- Collision detection fixes and follow-up bug fixes (_TheTermos_)
- Many many networking improvements (_sfan5_)

### Script API / Modding

- Log warning when `secure.enable_security` is disabled (_rubenwardy_)
- Exposed zoom key to the API (_appgurueu_)
- Add LevelDB auth database (_luk3yx_)
- Add vector functions useful for working with rotations (_NetherEran_)
- Add `minetest.is_creative_enabled` (_Wuzzy_)
- Add `minetest.on_authplayer` callback (_sorcerykid_)
- HUD: Implement scalable texts (_LoneWolfHT_)
- HUD: "off state" icon feature for statbars (_Wuzzy_)
- `set_fov`: Add time-based transitions (_ANAND_)
- Expose collision information to LuaEntity `on_step` (_sfan5_)
- Enforced type checks for registration fields (_SmallJoker_)
- Add server side translations capability (_pyrollo_)
- Fix alias handling of `minetest.get_content_id` (_sfan5_)
- Document inheritance of different noise seeds (_paramat_)
- Add default stack size setting (_nephele_)
- Play `player_jump` sound when player jumps (_Wuzzy_)
- HUD: Better waypoints and new image variant (_appgurueu_)
- CSM: Implement `minetest.sound_fade()` (_sfan5_)
- Error on invalid mapgen alias (_Wuzzy_)
- Add `allowed_mapgens` option in `game.conf` (_wt_)
- Lua API documentation clarifications (_Wuzzy_)
- `minetest.PLAYER_MAX_BREATH_DEFAULT` set to 10 (was 11) (_ANAND_)

### Misc / Build

- Android: Hide media from galleries etc (_rubenwardy_)
- Minimal development test [minimal] renamed to “Development Test” [devtest] (_Wuzzy_)
- Complete overhaul of Development Test, many new features added for testing the engine, more documentation added in README files, etc. (_Wuzzy_)
- Particle & ParticleSpawner code cleanup (_sfan5_)
- Add Prometheus support (_nerzhul_)
- Fix detection of in-place `path_locale` when `RUN_IN_PLACE=0` (_sfan5_)
- List `STATIC_LOCALEDIR` in "--version" (_sfan5_)
- Many code optimizations (_sfan5_)
- Wireshark dissector update (_sfan5_)
- Split and re-organize server object code (_nerzhul_)
- Automatic build improvements (_sfan5_, _nerzhul_)

### Minetest Game

- Rename “Dry Dirt” and related blocks to “Savanna Dirt” and similar (_paramat_)
- Added Wild Cotton: grows in savannas, drops Cotton Seeds (_paramat_)
- Sort items into correct categories (_An0n3m0us_)
- Tune cloud density variation (_paramat_)
- Fix broken Creative inventory search in translation (_sfan5_)
- Make Straw Stairs/Slabs usable as fuel (_Paul Ouellette_)
- New textures: Dry Shrub, Brake Rail (_Extex101_, _Hooded Ice_)
- Block particles when leaves decay, TNT explodes (_sfan5_)

## 5.1.0 → 5.2.0

Released on 5 April 2020.

### Client / Audiovisuals

- Fix alpha blending in texture modifiers (_Warr1024_)
- Make natural night light as bright as MT 0.4.16 (_paramat_)
- Shader fixes (_lhofhansl_, _SmallJoker_)
- Clean up font caching, fix bitmap fonts (_SmallJoker_)
- Waves generated with Perlin-type noise #8994 (_lhofhansl_)
- Attachments: Fix glitches after detach (_SmallJoker_)
- Let node 'place' and 'dug' sounds be heard by other players (_sfan5_)
- Fix weird looking liquid source (_Wuzzy_)
- Basic model shading (_dcbrwn_)
- Improve arm inertia animations (_kilbith_)

### GUI Improvements

- Add visual feedback for button states (_Df458_)
- Formspec: draw order and clipping for all elements (_DS_)
- Refactor internal button styling/rendering code (_Df458')_
- Formspec: Fix clicking on tooltip-obstructed elements (_DS_)
- Formspec: change cursor on fields and co. (_DS_)
- Various new formspec bug fixes (_SmallJoker_)
- Formspec: Add 9-slice background support to button elements (_Df458_)
- Formspec: add hypertext[] element (_pyrollo_)
- Formspec: `animated_image[]` (_Df458_, _kilbith_)
- Make clipping of formspec elements more consistent (_Df458_)
- Remove outdated `field_close_on_enter[]` warnings in element parameters (_SmallJoker_)
- Fix mouse events sent to wrong GUI elements when dragging (_sfan5_)
- Restore intuitive click-through behavior (_DS_)

### Enhancements

- Wear out tools on punch (_sfan5_)
- Tunnels: Completely disable generation when 'cave width' >= 10.0 (_paramat_)
- Randomwalk cave liquids: Remove deprecated 'lava depth' (_paramat_)
- Automatically enable the mod's dependencies in the world config menu (_HybridDog_)
- Clean up craft replacements docs (_pauloue_)
- Falling nodes: add missing support for light sources, most drawtypes, and paramtype2s (_Wuzzy_)
- Remove legacy flat-file map code and documentation (_random-geek_)
- Fix packet receiving in server and client (_sfan5_)
- Key settings: Cancel with escape, clear with delete (_SmallJoker_)

### Script API / Modding

- CSM: Introduce `get_modpath()` (_sfan5_)
- CSM: Remove non-functional `minetest.get_day_count()` (_sfan5_)
- Add z-index management to HUD (_pyrollo_)
- Add `table.key_value_swap` and table.shuffle (_HybridDog_)
- Map download: Escape ':' to '\_' (NTFS/FAT\* systems) (_Montandalar_)
- Settings: Add `get_flags` API for mapgen flags (_SmallJoker_)
- Improve `minetest.sound_play` with ephemeral sounds and player exclusion (_sfan5_)
- Reworked validity checks for entities (_sfan5_)
- Documentation: Add advice on lifetime of ObjectRefs (_sfan5_)
- Allow texture modifiers in hotbar textures (_Warr1024_)
- Nodes with torchlike drawtype and custom `visual_scale` now are rendered attached to surface instead of being centered (_Wuzzy_)
- Add documentation of VoxelArea 'ystride', 'zstride' (_paramat_)
- Lua API: Document HP, breath and damage limits (_SmallJoker_)
- Various documentation improvements (_Wuzzy_)
- CSM: Corrections to `client_lua_api.txt` (_sfan5_)
- Make `minetest.item_place_node` return position of placed node (_Bluebird_)
- Call `on_secondary_use` when object is right-clicked (_sfan5_)
- CSM: Various fixes (_sfan5_)
- Many pathfinder bugfixes and improvements (_Wuzzy_)
  - Fix failure to find path if start or end pos is over air (_Wuzzy_)
  - Fix very broken implementation of A\* search (_Wuzzy_)
  - No longer jump through solid nodes (_Wuzzy_)
  - Return nil if start or end pos is solid (_Wuzzy_)
- New `set_sky`, `set_sun`, `set_moon`, and `set_stars` (_Jordach_)
- Secure and document minetest.deserialize() (_luk3yx_)
- `minetest.get_content_id`: throw error for unknown nodes (_HybridDog_)

### Misc / Build

- Don't install fonts on `ENABLE_CLIENT=0` configurations (_sfan5_)
- Fix memory leaks in formspecs (_SmallJoker_)
- Android: build fixes & compat fixes (_MoNTE48_, _nerzhul_)
- Run luacheck in travis, add luacheck (_rubenwardy_)
- Android: fix cyrillic characters (_Maksim_)
- Various build issue fixes (Clang, Travis CI) (_sfan5_)

### Minetest Game

- Simple weather that varies the clouds (_paramat_)
- More realistic boat physics (_gang65_)
- Creative Inventory search: Item with exact match appears in first spot (_sfan5_)
- Creative Inventory: More visual tweaks (_Andrey2470T_)
- Improve player model animation (_An0n3m0us_)
- Increase water opacity (_lhofhansl_)
- Reset spawn position when bed is destroyed (_Desour_)
- Papyrus now appears in rainforest swamps as well (_paramat_)
- New/updated translations:
  - French (_DrHackberrry_)
  - Spanish (_runsy_)
  - Italian (_h4ml3t_)
  - Russian (_Andrey2470T_)
  - Swedish (_Aresiel_)
  - Malay (_MuhdNurHidayat_)
  - Chinese (_zaoqi_, _IFRFSX_)
- Bugfix: TNT mod crashed when entities disappeared during explosion (_sfan5_)
- Bugfix: Papyrus in savanna did not appear in 5.1.0, it has been added back (_paramat_)
- Bugfix: Screwdriver was able to rotate torches into weird positions (_An0n3m0us_)
- Bugfix: Players could be knocked back when attached (_sfan5_)

## 5.1.0 → 5.1.1

Released on 17 January 2020.

This release is based upon bugfixes from 5.2.0-dev.

### Client / Audiovisuals

- Fix player-bound sound playback (_SmallJoker_)
- Fix item eat sound not played if last item (_Wuzzy_)

### World / Server / Environment

- Formspecs: Reset version number on rebuild (_SmallJoker_)
- Rework packet receiving in ServerThread (_sfan5_)
- Fix `core.chat_format_message` crashes (_ClobberXD_)
- Fix spaces breaking `formspec_version[]` tag (_rubenwardy_)

### Misc / Build

- MacOS/BSD: Fix build issue due to conflicting s64 type definitions (_AMDmi3_)
- Fix find_path for newer jsoncpp installations (**vilhelmgray**)
- Update translations

## 5.0.1 → 5.1.0

Released on 12 October 2019.

### Map generator

- Mapgen Carpathian: Add optional rivers (_paramat_)
- Move more dungeon parameter selection to mapgens (_paramat_)
- Dungeons: Make multiple large rooms possible (_paramat_)

### Client / Audiovisuals

- Change pitch fly binding to 'P', add to change keys menu (_rubenwardy_)
- Android settings: Use 'simple' leaves instead of 'fancy' (_paramat_)
- Fix 3rd person selection range (_srifqi_)
- Make scrollbars' bar variable in size (_stujones11_)
- Damage: Play no damage sound when immortal (_SmallJoker_)
- Increase upper limit of `display_gamma` to 10 (_ClobberXD_)
- Optimize and unify mesh processing (_Vitaliy_)
- Re-order mapgens in mainmenu and 'all settings' mapgen selection (_paramat_)
- Scrollbars: Move directly to clicked pos if clicked into tray (_DS-Minetest_)
- Fix broken attachments on join (_SmallJoker_)
- Fix `inventory_overlay` for nodes without `inventory_image` (_DS-Minetest_)
- Better F6 profiler (_SmallJoker_)
- Fix minimap markers (_theviper121_)
- Textures: Load base pack only as last fallback (_SmallJoker_)

### Builtin (Lua / Media / Documentation)

- Add /help formspec for commands and privileges (_SmallJoker_)
- Lua API: Add link to Minetest Modding Book (_ClobberXD_)
- Force item entities out of solid nodes (_sfan5_, based on _Wuzzy_\`s work)
- Lua API: Various fixes (_DS-Minetest_, _SmallJoker_)
- Rename "private messages" to "direct messages" (_Calinou_)
- Automatically enable depending mods in the dialogue (_HybridDog_)
- All settings: Fix missing flags checkboxes (_srifqi_)

### World / Server / Environment

- Various network performance improvements (_osjc_)
- Force send a mapblock to a player (_sofar_)
- Revert ItemStacks being limited to `stack_size` (_ClobberXD_)
- Save forceloaded blocks file periodically (_Thomas Rudin_)
- Improve ABM time budget handling (_lhofhansl_)
- Group "immortal" also protects players from damage (_Wuzzy_)
- Optimize usage of `TOSERVER_GOTBLOCKS` packet (_sfan5_)
- Network: several bugfixes (_sfan5_)
- Mapgen::spreadLight performance improvement (_DS-Minetest_)
- Improve occlusion culling in corridors with additional check (_sfan5_)
- Inventory: Delay dirty lists, and send changes incrementally (_SmallJoker_)
- Other inventory bugfixes (_sfan5_, _SmallJoker_)
- Move debug.txt after it grows too big (_HybridDog_)
- Trigger `on_place` in many situations even if prediction failed (_DS-Minetest_)
- Punchwear (improved) (_sfan5_)

### Script API / Modding

- Add deprecation warnings for `ObjectRef:get/set_attribute` (_ClobberXD_)
- Nodedef 'drop' documentation: Improve (_paramat_)
- Mark tool filtering in node drop documentation as deprecated (_paramat_)
- Add node field to PlayerHPChangeReason table (_pauloue_)
- Don't call `on_hpchange` callbacks if HP hasn't changed (_ClobberXD_)
- Add `disable_jump` to liquids and ladders (_SmallJoker_)
- Add support for 9-sliced backgrounds (_rubenwardy_)
- Add compatible, consistent coordinate system to FormSpecs (_v-rob_)
- Document ObjectRef:remove under Lua entity (_ClobberXD_)
- Docs: Clarify where to check for `protection_bypass` (_SmallJoker_)
- Add vector.dot and vector.cross (_HybridDog_)
- Improve documentation of mapgen aliases (_paramat_)
- Remove debug.upvaluejoin to prevent leak of insecure environment (_fluxionary_)
- Fix previously crashing `minetest.get_craft_result()` (_pauloue_)
- Allow toolcaps to override the built-in times for `dig_immediate` (_sfan5_)
- Formspec styling using style[] (_rubenwardy_)
- Customizable chat message format (_ClobberXD_)
- Velocity modifiers for players (_sfan5_)
- Fix some issues with `minetest.clear_craft` (_pauloue_)
- Add function `minetest.read_schematic` (_paly2_)
- Formspecs: `formspec_version[]` element (_SmallJoker_)
- Per-player FOV overrides and multipliers (_ClobberXD_)

### Misc / Build

- Find PostgreSQL correctly (_adrido_)
- Add compatibility to vcpkg buildsystem (_adrido_)
- Android: Use system provided path for default TMPFolder setting (_stujones11_)
- Fix handling of --color and --worldlist command line arguments (_mmattes_)
- Unified OpenGL ES support (_sfan5_)

### Minetest Game

- Added dry dirt and dry dirt with dry grass. Grass never spreads on dry dirt (_paramat_)
- Savannas now have dry dirt and dry dirt with dry grass (_paramat_)
- Added steel bar door and steel bar trap door
- Added setting `enable_fence_tall` for fences that cannot be jumped over (_mbartlett21_)
- Firefly in a bottle can now be placed into vessels shelf
- Add flowing water sound
- Underground biomes extend much deeper, down to Y=-255
- Make the creative mod hand dig `dig_immediate` nodes fast (_paramat_)
- Add new TNT sounds (_TumeniNodes_)
- Add missing infotext to nodes (_An0n3m0us_)
- Added translation support
- Added German, French, Spanish and Italian translations (_Wuzzy_, _Hamlet_, _JDiaz_, _DrHackberry_)

## 5.0.0 → 5.0.1

5.0.1 was released on March 31, 2019.

- Fix detached inventory serialisation (_rubenwardy_)
- Fix texture rotation for wallmounted nodeboxes (_sfan5_)
- Fix build failing on some compilers (_rubenwardy_)
- Warn about issues with the `num_emerge_threads` setting (_paramat_, _sofar_)
- HPChange Reason: Fix issues with custom reasons (_rubenwardy_)
- Fix FreeBSD build by handling `std::time_t` properly (_rubenwardy_)
- Confirm registration GUI: Remove positional strings to fix Windows bug (_paramat_)
- Prevent multi-line chat messages server-side (_rubenwardy_)
- httpfetch: Disable IPv6 here too if requested by settings (_sfan5_)

## 0.4.16 → 5.0.0

Released March 4th

Note: 5.0.0 is based on 0.4.16, not 0.4.17. 0.4.17 was created by backporting changes from the development branch of 5.0.0

### Highlights

- Add online content repository (games, mods, modpacks, texture packs)
- Add Carpathian mapgen
- Automatic jumping
- Android: Rewritten controls. Add joystick and modify up on-screen buttons
- Mods and games can be translated
- Rename 'subgame' to 'game'
- Modding: More node drawtypes: disconnected nodeboxes (more ways to create connected blocks), `plantlike_rooted` (for underwater plants)
- Modding: Custom player collision box
- Modding: A great deal of new map generator features
- Modding: Allow to rotate entities in all 3 axes

### New Version Scheme

Basically, the leading 0 has been dropped. The [version scheme](/for-engine-devs/version-number) was thus changed from

```
ZERO.MAJOR.MINOR.PATCH

```

(note: PATCH was left out when it was 0) to

```
MAJOR.MINOR.PATCH

```

.

This was chosen for a few reasons:

- Shows that we aim to keep inter-version compatibility (which has mostly been the case since early 0.4.x)
- Allows us to release patch/bug fix only releases without adding a silly 4th number
- Doesn't imply that this version is the first stable/finished version as much as a 1.0.0 does.

For some context, read [the issue that resulted in this change](https://github.com/luanti-org/luanti/issues/6073).

### Breaking changes and deprecations

As a major release, 5.0.0 will break some mods written for 0.4.x versions. We tried to keep the breakages as small as possible whilst fixing long standing issues.

- Minetest now uses C++11 instead of C++03, make sure you have a compatible system (Windows Vista, Debian 8, Ubuntu 16.04, CentOS 7, macOS 10.7, …)
- Breaks network compatibility with 0.4.x. 0.4.x clients won't be able to connect to 5.x servers and vice-versa. Fix: update clients and servers to 5.x
- Attachment and player positions have been shifted by 1 node, to allow support for custom player selection boxes. Fix: update default and player_api, subtract `10` from any attachment positions, shift the origin of player models to the feet
- Formspec theming using prepended strings. This may cause wrong backgrounds to appear on formspecs. Fix: see [https://forum.luanti.org/viewtopic.php?f=18&t=20646](https://forum.luanti.org/viewtopic.php?f=18&t=20646)
- depends.txt and description.txt have been deprecated. Fix: use _description_, _depends_, and _optional_depends_ in mod.conf, game.conf, or texture_pack.conf instead
- modpack.txt has been deprecated. Fix: rename to or add _modpack.conf_.
- Use of deprecated methods such as _object:setpos()_ is now warned about. Fix: [replace them with correct functions](https://forum.luanti.org/viewtopic.php?f=18&t=20403)
- _player:get/set_attribute()_ is now deprecated. Fix: use _player:get_meta()_ instead
- _nodeupdate()_ was removed. Fix: replace with _minetest.check_for_falling_.
- Clouds API: changed speed param from 'y' to 'z'
- Player inventory list "hand" must now be manually initialized by mods (using _set_size_)
- Some client-side scripting functions were removed (see below)

Note that a function being _deprecated_ means that it still exists, but it may give a warning when used and may be removed in future. Deprecations are necessary to improve the consistency and efficiency of our API

### Features

#### Games

- Load a texture pack from the 'textures' subfolder of a game (_red-001_)

#### Map generators

- Mapgen: Add Carpathian mapgen (_Vaughan Lapsley_)
- Mapgen flags: Add 'biomes' global mapgen flag (_paramat_)
- Dungeons: Add Y limits in all mapgens (_paramat_)
- Dungeons: Add setting to prevent projecting dungeons (_paramat_)
- Biomes: Add vertical biome blend (_paramat_)
- Mgv7 floatlands: Add exponent parameter (_paramat_)

#### User interface

- Add online content repository (mods, texture packs, mod packs, games) (_rubenwardy_)
- Rename 'subgame' to 'game' (_paramat_)
- Allow enter to select items from combobox's list (_basicer_)
- Formspecs: Use mouse wheel to pick up and deposit single items (_HybridDog_)
- Add player marker to both minimap types (_nanoproject_)
- Delete world dialogue: Move buttons to avoid double click deletion (_srifqi_)
- Add a refresh button to the server list (_ThomasMonroe314_)
- Main menu: Change tabs to 'Start Game' and 'Join Game' (_ThomasMonroe314_)
- Add password confirmation on new player registration (_srifqi_)
- Adjust default console height (_Ezhh_)
- Add coloured terminal output (_HybridDog_)
- Add setting to display the itemstring after the tooltip in the inventory (_DTA7_)
- Make world creation menu automatically generate a random world name (_lisacvuk_)
- Change “Use” key name to “Special” (_TeTpaAka_)
- Make number of maximum displayed chat messages configurable (_Esteban_)

#### Controls

- Automatic jumping (_bendeutsch_)
- Add pitch move mode, toggle with L key
- Android: Rewritten controls. Add joystick and modify up on-screen buttons (_srifqi_)
- More keys are changeable in settings menu
- Make direct item selection keys freely bindable via minetest.conf (_Wuzzy_)

#### World / server

- Add setting for world start time, and change default to 5:15am (_JRottm_, _paramat_)
- Allow for getting world name and path separately on the command line (_Brian_)
- Add a server-sided way to remove color codes from incoming chat messages (_red-001_)
- Object limits: Allow objects to exist outside the set 'mapgen limit' (_paramat_)

#### Chat commands and privileges

- Add '/haspriv' chat command (_ClobberXD_)
- Add '/kill' chat command (_Wuzzy_)
- Remove 'zoom' privilege (zoom is now a player object property)
- Do not grant all privileges to the admin - changes game behavior (_lhofhansl_)

#### Graphics

- Fog effect when camera is inside cloud (_bendeutsch_)
- Camera: Add and improve arm inertia (_kilbith_)
- Update light decoding table size (_numberZero_)
- New lighting curve (_numberZero_)
- Add setting for near plane distance (_basicer_)
- Add Android drivers to the `video_driver` drop-down menu (_Wayward1_)

#### Sounds

- Sounds: Add falling node sounds (_sofar_)
- Emit liquid sound if the player walks in liquid (_juhdanad_)
- Play damage sound on player death (_paramat_)
- Add mute setting (toggled by the mute key and in the volume menu) (_DTA7_)

#### Technical

- Add support for authentication in an SQLite database (_bendeutsch_)
- Add an optional readonly base database (_lhofhansl_)
- Add crossview support (_otdav33_)
- Optionally extend the active object in a players camera direction (_lhofhansl_)
- Implement ability to set client node dig prediction result (_sofar_)

#### Misc.

- Add check to pause game on lost window focus (_rubenwardy_)
- Load files from subfolders in texture packs (_numberZero_)
- Make HUD status messages translatable (_Wuzzy_)

### Modding

- Add `on_mods_loaded` callback (_nerzhul_)
- MetaDataRef: Add contains() and get() (_rubenwardy_)
- Allow dumping userdata (_HybridDog_)
- Hint at problematic code when logging deprecated calls (_sfan5_)
- Add minetest.rgba function that returns ColorString from RGBA or RGB values (_Gael-de-Sailly_)
- World config: Add modpack descriptions (_HybridDog_)
- `get_node_drops`: Make empty drop return empty table (_tenplus1_)
- Add `core.remove_detached_inventory` (_SmallJoker_)
- Add `disable_repair` group to prevent tool repair (_Wuzzy_)
- `clear_craft`: Return false if recipe not found, don't throw error (_paramat_)
- Fix string.split returning an empty table if string starts with separator (_pyrollo_)

#### Formspecs / HUD

- Formspecs: Add tooltip element for area (_rubenwardy_)
- Formspecs: Allow setting alpha value for the box[] element (_Thomas--S_)
- Formspecs: Add an auto vertical scrollbar to the textarea (_adelcoding1_)
- Formspecs: Add options to set background color and opacity (fullscreen mode + default mode) (_nerzhul_)
- Formspecs: textarea with scrollbar improvements (_adrido_)
- HUD: Make `hud_get` return alignment, offset and size. (_lisacvuk_)

#### Server-side

##### Metadata

- Give games the ability to disallow specific mapgens (_Ezhh_)
- Load mod dependencies and description from mod.conf (_rubenwardy_)

##### User interface

- Add client-side translations. (_Ekdohibs_)
- Add formspec theming using prepended strings (_rubenwardy_)
- Zoom: Set zoom FOV per-player using a player object property (_paramat_)
- Zoom: Move enabling zoom to a new player object property (_paramat_)
- Minimap: Add new HUD flag for minimap radar mode (_paramat_)

##### Items

- Allow overriding tool capabilities through itemstack metadata (_raymoo_)
- Set placer to nil instead of a non-functional one in `item_OnPlace` (_DTA7_)
- Overlays for wield and inventory images (_juhdanad_)
- Make dropped items colorable (_juhdanad_)
- Automatic item and node colorization (_juhdanad_)
- Add eat sound (_Wuzzy_)

##### Nodes

- Add slippery group for nodes (players/items slide) (_Wuzzy_)
- Connected Nodeboxes: Add 'disconnected' boxes (_Thomas--S_)
- Add callback to preserve node metadata as item metadata (_ashtrayoz_)
- Add `plantlike_rooted` drawtype (_numberZero_)

##### Objects/entities

- ObjectRef: Add `add_velocity()` (_HybridDog_)
- Allow damage for attached objects, add attach/detach callbacks (_SmallJoker_)
- Optional alpha channel support for entities (_stujones11_)
- Add `static_save` property to luaentities to not save them statically. (_orwell96_)
- Object properties: Add 'glow', disables light's effect if negative (_basicer_)
- `core.get_objects_inside_radius`: Omit removed objects (_HybridDog_)
- Make entity selection and collision boxes independently settable (_stujones11_)
- Add set_rotation/get_rotation to rotate entities in all 3 axes

##### Players

- Customizable max health and max breath for players (not persisted between server restarts) (_SmallJoker_)
- Add reasons to `on_dieplayer` and `on_hpchange` (_rubenwardy_)
- Add `minetest.is_player` (_HybridDog_)
- Add `on_grant` and `on_revoke` callbacks (_rubenwardy_)
- Player eye height: Make this a settable player object property (_paramat_)
- Step height: Add as a player object property (_paramat_)
- Player collisionbox: Make settable (_TeTpaAka_)
- Add player inventory callbacks (_SmallJoker_)

##### Mapgen

- Biomes: Add `get_biome_name(biome_id)` API (_paramat_)
- Biomes: Add `min_pos'/'max_pos` xyz biome limits (_paramat_)
- Biomes: Add decoration flags for underground decorations (_paramat_)
- Biomes: Add function to deregister single biomes (_zeuner_)
- Biomes / dungeons: Add biome-defined dungeon nodes (_paramat_)
- Biomes / cavegen: Add definable cave liquid for a biome (_paramat_)
- Biomes: Add 'get heat', 'get humidity', 'get biome data' APIs (_paramat_)
- Biomes/decorations/ores: Make relative to `water_level` setting (_paramat_)
- Place schematic (on vmanip): Enable use of 'place center' flags (_paramat_)
- Ores: Add stratum ore (_paramat_)
- Stratum ore: Add option for a constant thickness stratum (_paramat_)
- Stratum ore: Allow use with no noise for simple horizontal strata (_paramat_)
- Simple decorations: Add `param2_max` parameter for random param2 (_paramat_)
- Schematic decorations: Add `place_offset_y` placement parameter (_paramat_)
- Decoration API: Add lightweight ability to have complete coverage (_paramat_)
- Mgv6: Remove incorrectly defined and unused 'volume nodes' (_paramat_)
- Mgv7: Add `mount_zero_level` parameter (_paramat_)
- Mgv7: Add option to repeat surface biomes in floatlands (_paramat_)
- Mgvalleys: Make river depth variation and humidity drop optional (_paramat_)
- CavesRandomWalk: Make `lava_depth` a mapgen parameter (_paramat_)
- Gennotify: Add `minetest.get_decoration_id` API (_paramat_)

##### World

- Spawn level: Add `get_spawn_level(x, z)` API (_paramat_)
- Add `minetest.bulk_set_node` call + optimize `Environment::set_node` call (_nerzhul_)
- Implement `minetest.register_can_bypass_userlimit` (_nerzhul_)

##### Chat

- Make the server status message customizable (_SmallJoker_)
- Allow the join/leave message to be overridden by mods. (_red-001_)

##### Protection

- `is_area_protected`: Rename from `intersects_protection` (_SmallJoker_)
- `Intersects_protection()`: Move from Minetest Game to builtin (_paramat_)

##### Graphics and sounds

- Real global textures (_numberZero_)
- Clouds API: change speed from 'y' to 'z', ColorSpecs in Lua docs (_bendeutsch_)
- In-cloud fog: Strengthen effect when small view range is used (_lhofhansl_)
- Sound: Add pitch option (_Rui-Minetest_)

##### Utility features

- Allow 'default' parameter in `settings:get_bool` function (_Jordan Irwin_)
- Add `on_auth_fail` callback (_red-001_)
- Make `core.auth_table` private and structure builtin/auth.lua (_sfan5_)
- Add sha1 to lua utils. (_basicer_)
- Remove nodeupdate and `nodeupdate_single` (_Rui-Minetest_)
- Expose getPointedThing to Lua (_juhdanad_)
- Helper methods for hardware colorization (_juhdanad_)
- httpfetch: Enable gzip support (_sfan5_)
- Rename hasprivs command to haspriv (_ezhh_)

#### Client-side

- Add flavour limits controlled by server (_nerzhul_)
- Disallow exploitable client-side mod functions by default (_paramat_)
- Rename CSM flavours to restrictions (_SmallJoker_)
- Remove screenshot API (_red-001_)
- Don't Load the package library (_red-001_)
- Remove `on_connect` callback (_red-001_)
- Add functions to create particles and particle spawners. (_red-001_)
- Don't load the IO library. (_red-001_)
- Add a way to get current locale from CSM (_lisacvuk_)
- Add callback on open inventory (_Dumbledor_)
- Implement mod communication channels (_nerzhul_)
- Create a filesystem abstraction layer for CSM and only allow accessing files that are scanned into it. (_red-001_)
- Add function to get player privileges (_red-001_)

### Bug fixes and small improvements

Also a big thanks to paramat, ClobberXD, pauloue, gituser2194, lhofhansl, ashtrayoz, Wuzzy, and Ezhh for their contributions to documentation, in no particular order.

#### Critical

- Fix crash caused by Lua error during startup (_red-001_)
- `Pointed_thing_to_face_pos`: Avoid crash when player is inside a node (_paramat_)
- Fix segfault in player migration and crash in `log_deprecated` (_SmallJoker_)
- Fix crash guiConfirmRegistration quit menu (_Vincent Glize_)
- Fix built-in inventory list crash when size = 0 (_SmallJoker_)
- Fix a crash or random memory leak when resetting saved environment variable in `test_servermodmanager.cpp` (_nerzhul_)
- Fix crash on `can_bypass_userlimit` returning non-boolean (_rubenwardy_)
- Fix error if setting `menu_last_game` is not a valid game (_nOOb3167_)
- Builtin: Fix `handle_node_drops` crash with nil digger (_SmallJoker_)
- Fix issue Minetest crash when custom font path is not exist (_srifqi_)
- Thread: fix a crash on Windows due to data race condition on `Thread::m_start_finished_mutex` (_nerzhul_)
- Fix crash on revocation of removed privilege (_rubenwardy_)
- Fix crash when using --go in command line (_juozaspo_)
- Fix crash due to missing pointer validation (_nerzhul_)
- Fix `get_server_status()` segfault due to uninitialized `m_env` (_rubenwardy_)

#### Build

- Fix many Android build issues (_nerzhul_)
- Fix i386 bit build at OpenBSD (_mazocomp_)
- Fix compile error in OpenBSD (_jcalve_)
- Fix MSVC compiling annoyances (_adrido_)
- directiontables: Fix MSVC compiler error (_adrido_)
- Fix MacOS builds (_Pavel Puchkin_)
- Travis-ci build: fix osx jpeg installation failure, git ambiguous argument error (caused by merging commits) and add a workaround for travis commit range bug (_juozaspo_)

#### System-specific

- Provide Xorg/net wm process ID (_thoughtjigs_)
- Make os.tempfolder work correctly for MinGW & MSVC (_nOOb3167_)
- Print error when HOME is not set (_Midgard_)
- MacOS: don't require X11 libraries during compilation (_D Tim Cummings_)
- Prevent Android from automatically locking display (_Wayward1_)
- Windows: Cpack wix installer (_adrido_)

#### Rendering

- Fix liquid bottoms not being rendered (_numberZero_)
- Fix liquid post effect colour behavior in third person view (_red-001_)
- Fix dark liquids (_numberZero_)
- Use crack animation on all tile layers (_juhdanad_)
- Smoothed yaw rotation for objects (_SmallJoker_)
- Disable shaders GUI on unsupported drivers (_numberZero_)
- Fix missing ignore textures (_HybridDog_)
- Make sure color returns to normal after a damage flash (_lhofhansl_)
- Camera: Improve subpixel movement (_SmallJoker_)
- Camera: Fix wieldmesh glitch after teleporting (_kilbith_)
- `upright_sprite`: Fix texture position for players (_SmallJoker_)
- Fix node-nodebox lighting difference in direct sunlight (_numberZero_)
- Fix ambient occlusion and dark lines at mapblock borders (_numberZero_)
- Don't recalculate statustext initial color every time & review fixes (_nerzhul_)
- Clear colors when reading property info. Set vertex colors on `upright_sprites.` (_basicer_)
- Fix items turning black (_numberZero_)
- Fix dropped item look (_HybridDog_)
- Fix item and wield meshes (_numberZero_)
- Do not scale texture unless necessary. (_lhofhansl_)
- Avoid filtering low-res textures for animated meshes (incl. players) (_lhofhansl_)
- Reduce server FOV with forward speed (_lhofhansl_)
- ParticleSpawner::step cleanup and rotation fix (_SmallJoker_)
- Fix incorrect buffer size calculation on creation of HUD status messages (_rubenwardy_)
- Particles: Do not add digging particles for airlike nodes (_SmallJoker_)
- Fix animation `frame_speed` and blend loosing precision (_sapier_)
- Fix undefined behavior in arm movement when dividing by zero (_nerzhul_)
- Fix render order of overlays (_juhdanad_)
- Particles: Make collision with objects optional (_paramat_)
- Fix stretched stars bug, change render order (_Aspen_)
- Software inventorycube (_Vitality_)
- Light curve: Simplify and improve code, fix darkened daytime sky (_Vitality_)
- Smooth lighting: Fix light leaking through edge-connected corners (_DTA7_)
- Darkness detection: Reduce chance of false positives darkening the skybox (_lhofhansl_)
- Fix sky objects not rendering with OpenGL ES (_stujones11_)
- Night clouds: Boost brightness for a moonlit appearance (_paramat_)
- Night sky: Fix brightness threshold for applying night colours (_paramat_)

#### User interface

- Fix debug and info text being the wrong color (_rubenwardy_)
- Fix tooltip colors specified by formspec part (_rubenwardy_)
- Make RTT display (F5 information) working correctly again (_HybridDog_)
- Don't show Android edit dialog when tapping read-only field (_srifqi_)
- Make sounds stop playing when entering game or mainmenu (_nOOb3167_)
- Fix dancing text for formspec input fields (_numberZero_)
- Chat: Remove prompt history duplicates (_SmallJoker_)
- Statbars: fix incorrect half-images in non-standard orientations (_Ekdohibs_)
- Advanced settings: Add range check for float type (_srifqi_)
- Move the nametag back to the top of the player (_TeTpaAka_)
- Chat: Move chat text down to not overlap 3rd line of debug text (_paramat_)
- Fix console resize issue when changed game window (_Ezhh_, _Zeno-_)
- Android buttons: Inset 'rare controls', inset and resize 'gear icon' (_paramat_)
- Main menu: Clean up and improve advanced settings dialogues (_SmallJoker_)
- Escape special characters when searching the server list (_ChimneySwift_)

#### Formspecs

- Mitigate formspec exploits by verifying that the formspec was shown to the user by the server. (_red-001_)
- Make container[] support fractional offsets (_SmallJoker_)
- Remove accidental empty 'quit' field (_SmallJoker_)
- Unify textarea and field parsing functions, fix wrong fallback text (_SmallJoker_)
- Formspec verification: Fix `show_formspec` inside callbacks (_SmallJoker_
- Fix wrong scrolling of formspec input fields (_numberZero_)
- Inventory: Restrict access from too far away (_SmallJoker_)
- Fix mousewheel behavior in textarea (_shivajiva101_)
- Fallback to 'label' in readonly textarea[] (backwards compatible) (_SmallJoker_)
- Fix invalid background warning (_SmallJoker_)
- Fix text clipped by scrollbars (_random-geek_)

#### Performance

- Optimized MapBlock mesh generation (_lhofhansl_)
- Optimize ABM checks (_lhofhansl_)
- Fix bugs in networking which caused a performance loss (_lhofhansl_)
- Builtin auth handler: Speed up file writing (_SmallJoker_)
- Huge LBM lookup performance improvement on mapblock loading (_nerzhul_)
- Fix last performance-type-promotion-in-math-fn problems (_nerzhul_)
- Optimize a little bit isBlockInSight, adjustDist & collisions (_nerzhul_)
- `line_of_sight`: Improve using VoxelLineIterator (_juhdanad_)
- Cache server config settings. (_lhofhansl_)
- Very little performance fix on correctBlockNodeIds (_nerzhul_)
- Don't search for locale folders if gettext is disabled (_adrido_)

#### Networking

- Use server's zoom fov for distant world loading. (_lhofhansl_)
- Fix `ipv6_server=true` not accepting IPv4 connections on Windows (_sfan5_)
- Fix narrow/utf8 difference in incoming/outgoing messages (_numberZero_)
- Fix `day_night_ratio_do_override` not being initialized server-side (_rubenwardy_)
- Fix attached particle spawners far from spawn (_raymoo_)
- Server: affect `bind_addr` on constructor instead of start() (_nerzhul_)
- Network: Fix logging into older worlds with base64 hashes (_SmallJoker_)
- Server: Calculate maximal total block sends dynamically (_SmallJoker_)
- Have the server send the player list to the client (_red-001_)

#### Chat commands and privileges

- Check if player exists on use of /privs (_ClobberXD_)
- Reduce block load glitches (_lhofhansl_)
- Make the /shutdown command work properly (_dopik_, _SmallJoker_)
- Prevent /spawnentity from spawning unknown entity (_Wuzzy_)

#### World

- Fix blocks written by VManip not being marked as modified (_sfan5_)
- Set range of blocks to retrieve per roundtrip to 2. (_lhofhansl_)
- Retrieve a small cone of blocks in the direction of the players velocity. (_lhofhansl_)

#### Map generator

- Change mapgen order to ores > dungeons > decorations (_paramat_)
- Biome API: Fix absent water decorations and dust, in deep water (_paramat_)
- Biome-defined dungeon nodes: Use faster biome calculation (_paramat_)
- Biomemap: Avoid empty biomemap entry to fix failing biome dust (_paramat_)
- Biomes: Fix vertical biome blend (_paramat_)
- Biome dust node: Only place on 'walkable' cubic non-liquid drawtypes (_paramat_)
- Biome generation: Fix layers of 'filler' nodes at biome y limits (_paramat_)
- Mgv5: Make spawn position search more reliable (_paramat_)
- Mgv6 mudflow: Avoid partially removed stacked decorations (_paramat_)
- Mgv7: Raise spawn point by 1 node for no mountain case (_paramat_)
- Mgv7: Avoid rivergen removing mod-placed nodes when overgenerating (_paramat_)
- Mgv7: Avoid divide-by-zero errors (_paramat_)
- Mgv7: Fix undefined `float_mount_height` (_paramat_)
- Mgfractal: Improve spawning behavior(_paramat_)
- Mgv5/v7/fractal: Add `large_cave_depth` parameter to replace fixed value (_paramat_)
- Fix Mapgen Valleys getSpawnLevelAtPoint() (_Treer_)
- Vein ore: Fix bug caused by changing perlinmap Y size (_paramat_)
- Schematic decorations: Fix placement bug when centered and rotated (_paramat_)
- Simple decorations: Make `place_offset_y` usable with simple decorations (_paramat_)
- Dungeons: Fix duplication of y limit parameters (_paramat_)
- Dungeons: Mostly fix missing stair nodes (_paramat_)
- Dungeons: Avoid generation in multiple liquid nodes and 'airlike' (_paramat_)
- Dungeons: Use biome `node_stone` if normal stone types not detected (_paramat_)
- Cavegen: Fix errors when getting biome outside mapchunk (_paramat_)
- Cavegen: Re-order generation to fix cavern bug (_paramat_)
- Cavegen: Avoid unsupported biome 'top' or 'filler' nodes (_paramat_)
- valleys mapgen: Fixed submarine valleys shape (_Gael-de-Sailly_)
- settingtypes.txt: Fix valleys dungeon ymax error (_paramat_)
- L-system: Fix leaves cutting through stems (_HybridDog_)

#### Items and nodes

- Pointed thing to face pos: Use 'eye height' object property (_paramat_)
- Ensure no item stack is being held before crafting (_Luis Cáceres_)
- `core.rotate_node`: Run callbacks like with any regular placed node (_SmallJoker_)
- Don't try to craft a non-existent item (_Esteban_)
- Fix rotated node placement (_tenplus1_)
- Item drop: Tune to land exactly 2 nodes away with level view (_paramat_)
- Check `item_drop` amount client-side (_rubenwardy_)
- Fix Android node selection distance (_juhdanad_)
- Safe digging and placing (_bendeutsch_)
- Fix for empty key/value when reading item string with wear but no metadata (_Jesse McDonald_)
- Inventory: Fix wrong stack size behavior and item loss (_SmallJoker_)

#### Objects/entities

- Fix objects colliding with their children (_SmallJoker_)
- `core.spawn_falling_node`: Keep metadata (_SmallJoker_)
- Collision engine: Collide with 'ignore' nodes (_paramat_)
- falling.lua: Delete falling node entities on contact with 'ignore' (_paramat_)
- Position entity nametags relative to selection-box (_stujones11_)
- Damage: Remove damage ignore timer (_SmallJoker_)
- Item entities: Enable item collision detection for sudden movement (_DTA7_)
- Ease selection of entities behind nodes (_SmallJoker_)
- Object properties: Fix loss of custom selectionbox when it's not updated (_SmallJoker_)
- GenericCAO: Fix light position for non-players, remove deprecated initialization code (_SmallJoker_)
- GenericCAO: Fix dark model below y = 0 (_paramat_)
- CAO footstep sounds: Reduce gain to balance volume (_paramat_)

#### Players

- Make player liquid speed independent of FPS (_paramat_)
- Run detach callbacks on player leave (_SmallJoker_)
- Localplayer: Fix `disable_jump` effect and getStandingNodePos() (_SmallJoker_)
- Android stepheight: Only increase if 'touching ground' (_paramat_)
- Respect object property `hp_max` field for players (_SmallJoker_)
- Do not add base position to player selection box (_stujones11_)
- Abort if `static_spawnpoint` is an invalid setting instead of just giving an error log (_HybridDog_)
- Fix error not printed to console when no name is provided (_juozaspo_)
- Fix player coordinate rounding in collisionMoveSimple() (_JRottm_)
- Sneak: Stripped down version (_SmallJoker_)
- (Re)spawn players within `mapgen_limit` (_paramat_)
- Stop autoforward on BACKWARD key-press (_tukkek_)
- Apply physics overrides correctly during anticheat calculations (_sfan5_)

#### Modding

- Fix builtin Lua function os.tempfolder (_nOOb3167_)
- Fix isNan check for `object:set_yaw(..)` (_nerzhul_)
- Fix LuaJIT include directory not being found (_rubenwardy_)
- Check argument types inside MetaDataRef Lua API (_sfan5_)
- Fix buffer parameter not working in `LuaPerlinNoiseMap::l_getMapSlice()` (_pgimeno_)
- Fix naming conventions of noise userdata (_rubenwardy_)
- Fix rounding error in `g/set_node` caused by truncation to float (_rubenwardy_)
- Vector fun
- CAO footstep sounds: Reduce gain to balance volume (_paramat_)
- Fix default item callbacks to work with nil users (_raymoo_)
- `on_death`: Fix callback number of pushed arguments (_SmallJoker_)
- Fix `core.wrap_text` and make its behavior consistent with the docs (_sfan5_)
- Trigger `on_rightclick` regardless on the formspec meta field (_SmallJoker_)
- LBM: use range based for and fixed a loop variable overloading in applyLBMs (_nerzhul_)
- Fix deserialization of ItemDefinition (_Rui-Minetest_)
- Plantlike meshoptions: Fix inverted random vertical offset (_numberZero_)
- Player hand list: require init by mods (_SmallJoker_)

#### Debug

- Fix missing logs from warning stream (or similar) (_HybridDog_)
- Fix off-by-one in log output line length (_pgimeno_)
- Profiler: Fix var args not being passed to callback register function (_rubenwardy_)

#### Internal / code quality

- Add a MSVC / Windows compatible snprintf function (_nOOb3167_)
- Fix memory leaks in mod storage (_nerzhul_, _red-001_)
- Fix rtt >= 0.0f assertion and `free_move` crash (_SmallJoker_)
- Global new() or grab() to be managed in constructor only (_JDCodeIt_)
- Node resolver: Make error on fallback optional, disable for mapgen aliases (_paramat_)
- FOV: Raise lower limit to avoid zoom-loading of distant world (_paramat_)
- Fix many issues reported by clang-tidy (_nerzhul_)
- Fix various clang-tidy reported performance-type-promotion-in-math-fn (_nerzhul_)
- Selected ItemStack: Reduce black magic and improve the stack swapping behavior (_SmallJoker_)
- `core.rotate_node`: Do not trigger `after_place_node` (_SmallJoker_)
- Sound: fix static initialization order dependency by not having one (_nOOb3167_)
- Fix various Client class functions not marked as override (virtual) (_nerzhul_)
- Guard sound manager initialization with `enable_sound` (_nOOb3167_)
- Fix an alone if to be with a missing else (_nerzhul_)
- Drop texture file list cache (_numberZero_)
- Variable name fix + structure creation unrolling in lighting code (_nerzhul_)
- getv3intfield: Fix logic of return bool (_paramat_)
- Generate Notifier: Clear events once after all 'on generated' functions (_paramat_)
- Fix Wstringop-overflow warning from util/srp.cpp (_HybridDog_)
- Tool getDigParams: Fix selecting the best fitting time (_HybridDog_)
- Fix undefined behavior on getting pointer to data in empty vector (_nOOb3167_)
- Use Irrlicht's mesh cache for animated meshes. (_lhofhansl_)
- Shut down mapgen threads before other shutdown tasks (_raymoo_)
- Allow zoom to actually show more data. (_lhofhansl_)
- Make use of safe file writing in auth handler (_sfan5_)
- Fix inventory drag drop flag (_asl97_)
- Fix `strict_protocol_version_checking` functionality after ee9a442 (_SmallJoker_)
- Fix empty legacy meta being persisted (_rubenwardy_)
- Lint fix on localplayer.h (_nerzhul_)
- Sort box corners correctly (_Thomas--S_)
- Remove unused Map::getDayNightDiff + fix one undefined variable in mapblock.cpp (_nerzhul_)
- Check node updates whether the blocks are known (_SmallJoker_)
- Really delete things in fs::RecursiveDelete (_Vitality_)
- Disable HW stereo for IrrLicht 1.9 (_numberZero_)

#### Misc.

- Fix for translating empty strings (_minduser00_)
- Positional sound: Limit volume when closer than 1 node (_paramat_)
- Change the server description after a search (_Dumbledor_)
- Fix no sound bug (_Rui-Minetest_)

### Other / Misc

- Version scheme change: 0.5.0 -> 5.0.0 (_nerzhul_)
- Move ASCII art to std::cerr, to remove it from logs (_rubenwardy_)
- PlayerSettings struct for player movement code (_bendeutsch_)
- Client eventmanager refactor (_nerzhul_)
- Update mesh collector and move it to a separate file (_numberZero_)
- Add Voxelarea unittests (_nerzhul_)
- VoxelArea: add\_{x,y,z,p} must be static (_nerzhul_)
- Cleanup in flat lighting (_numberZero_)
- Node definition manager refactor (_juhdanad_)
- Move 'setlocale' from Lua to C++. (_red-001_)
- Rewrite rendering engine (_numberZero_)
- Improve the path select GUI (_red-001_)

## 0.4.17.1 → 0.4.17.2 (Android Only)

Patched 0.4.17.1 to fix some Android crashes, released on June 28, 2018.

- Android: Use correct temporary path (_stujones11_)
- Fix MurmurHash implementation to really be unaligned (_sfan5_)
- Fix crash caused by Lua error during startup (_red-001_)
- Fix buffer overrun in SRP (_red-001_)
- Android: use c++\_shared library instead of c++\_static (_nerzhul_)
- Android: Fix android tools version used to build MT (_nerzhul_)

## 0.4.17 → 0.4.17.1

Patched 0.4.17 to fix a crash, released on June 10, 2018.

- Correct character encoding for chat_send_player and chat_send_all
- Fix crash caused by log_deprecated and the use of deprecated functions
- Fix crash on pause menu when pressing up/down keys
- Android build system fixes

## 0.4.16 → 0.4.17

Backported release containing only bug fixes and small features. 0.4.17 was released on June 3, 2018.

### Feature

- Builtin auth handler: Speed up file writing
- LBM lookup performance improvement on mapblock loading
- Minetest ASCII art: Move from actionstream to rawstream
- CollisionMoveSimple: Collide with 'ignore' nodes
- Allow objects to exist outside of 'mapgen limit' (mapblocks may exist)
- Find nodes in area (under air): Raise volume limit
- Allow dumping userdata using dump()
- Add minetest.is_player API function
- Refine movement anticheat (again)
- Apply physics overrides correctly during anticheat calculations
- Shut down mapgen threads before other shutdown tasks
- Show script source of deprecated function calls
- Inventory: Restrict access from too far away (anticheat)
- Improve Settings tab button alignments
- Add minetest.sha1 util API function
- Avoid filtering low-res textures for animated meshes
- Add setting for near plane distance to improve performance
- Footstep sounds: Reduce gain to balance volume
- Positional sound: Limit volume when closer than 1 node
- Leveled nodebox: Change levels from 1/63rds to 1/64ths
- ClientInterface: user limit checking function
- Make dropped items colorable
- Trigger on_rightclick regardless on the formspec meta field
- Clarify "Full viewing range" key message
- Rework new sneak code, minimize
- Tile material: Opaque textures by default to prevent xray effect
- Automatic item and node colorization
- find_nodes_in_area: Extend maximal count to U32_MAX
- Add server option to remove color codes from chat messages

### Bug fixes and Improvements

- Various build fixes (LuaJIT not found, OpenBSD)
- Dungeons: Mostly fix missing stair nodes
- Cavegen: Fix variable typo that broke mgvalleys large cave distribution
- Prevent translating empty strings
- upright_sprite: Fix texture position for players
- core.rotate_node: Do not trigger after_place_node for mod compatibility
- macOS: don't require X11 libraries during compilation
- Generate Notifier: Clear events once after all 'on generated' functions
- Fix liquid post effect colour behavior in third person view
- Delete world dialog: Move buttons to avoid double click deletion
- Fix /shutdown countdown parameter
- Check argument types inside MetaDataRef Lua API
- Fix "Ignoring CONTENT_IGNORE redefinition" warning
- dropped items and falling nodes: Delete in 'ignore' nodes
- Move setlocale from Lua to C++
- Fix off-by-one in log output line length
- Fix buffer parameter not working in getMapSlice()
- Fix rounding error in g/set_node caused by truncation to float
- Fix dancing text in text input fields
- Fix undefined behavior on getting pointer to data in empty vector
- Fix wrong scrolling in text areas
- Builtin: Fix handle_node_drops crash with nil digger
- Damage: Remove damage ignore timer due to abuse potential
- Ensure no item stack is being held before crafting
- Several documentation additions, improvements
- core.rotate_node: Run callbacks like with any regular placed node
- Biome dust node: Only place on 'walkable' cubic non-liquid drawtypes
- Make use of safe file writing in auth handler
- Add minetest.safe_write_file() API function
- Fix issue Minetest crash when custom font path is not exist
- Fix Settings tab formspec alignment
- Do not scale texture unless necessary
- httpfetch: Enable gzip support
- Fix day_night_ratio_do_override not being initialized server-side
- Fix default item callbacks to work with nil users
- Prevent from crafting non-existent, unknown items
- Profiler: Fix var args not being passed to callback register function
- Unknown nodes: Provide position on interact
- Fix attached particle spawners far from spawn
- Localplayer: Fix disable_jump effect and standing node position
- Fix blocks written by vmanip not being marked as modified
- Set placer to nil instead of a non-functional one in item_OnPlace
- Fix Rotate Node Placement
- ServerEnv: Clean up object lifecycle handling (item deletion)
- Fix the core.wrap_text function
- Fix empty legacy meta being persisted
- Statbars: fix incorrect half-images in non-standard orientations
- Android stepheight: Only increase if 'touching ground'
- Fix Android node selection distance
- serialize: use a temporary for SerializeException
- Fix player coordinate rounding in collisionMoveSimple()
- Various crash and error fixes
- Fix for empty key/value when reading item string with wear but no metadata
- Fix render order of overlays
- Fix console resize issue when maximising game window
- Fix console not being properly resized after window size changed
- Verify HudSetParams input when hotbar textures are set
- (Re)spawn players within 'mapgen_limit'
- Fix sending color codes to clients that don't support them

## 0.4.15 → 0.4.16

0.4.16 was released on June 3, 2017.

- Minimum version supported is now 0.4.11

### Features

- Add 2D sheet animations for nodes (_sfan5_)
- Drop client side chat prediction. No more messages shown to chat when you talk and you are disconnected. (_red-001_)
- Add particle animation, glow (_sfan5_)
- Server list: add ping indicators (_kilbith_)
- Server side occlusion culling (_lhofhansl_)
- New custom progress bar (you can customize it with texture packs) (_kilbith_)
- Implement delayed shutdown for server owners: /shutdown 60 => shutdowns in 1 min /shutdown -1 cancels it (_nerzhul_)
- Add support for requesting a reconnect and changing the shutdown message to /shutdown (_red-001_)
- Add a mapblock cache in MeshUpdateQueue to improve client rendering performance (_celeron55_)
- Player data can now be into database. This is an important change, players to files are always supported for this release but deprecated. Files backend for players will be removed in a future release. See [Database backends](/database-backends) for compat matrix and migration steps. (_nerzhul_)
- Sounds: add fading sounds (_Bremaweb, krock_)
- Save automatically window size when modified. This behavior can be disabled in client settings (_nerzhul_)
- Add cancel button to password change formspec (_red-001_)
- Improve pause menu with more user friendly information and update keys dynamically depending on your configuration (_red-001_)
- Merge singleplayer & server tab on desktop clients (_octacian_)
- Add /clearinv chat command (_octacian_)
- Add keyword-based search to server-list and advance settings (_red-001, rubenwardy_)
- Add hardware-based itemstacks and node coloring (_juhdanad_)
- Undersampling which should make Minetest run better on low end devices (_numberZero_)

### Cheat fixes

- Breath cheat is now definitively fixed. Hacked clients cannot ignore breath anymore. (_nerzhul_)
- Fix node damage cheat. They are now calculated server side. Hacked clients cannot ignore node damages anymore (fire, lava, cactus...). (_nerzhul_)
- Disallow dropping items while dead. (_sfan5_)
- Calculate maximum interact distance from wielded tool. (_sfan5_)

### Bug fixes and Improvements

- Little antispam fix (_nerzhul_)
- Fix a little bit fog calculations (_lhofhansl_)
- Fix player deletion problem when too many objects are in a mapblock (_nerzhul_)
- Better block sending priorities to send map to players (_???_)
- Sneaking changes (_krock, paramat, sfan5_)
- Huge ABM handling code performance improvement (_nerzhul_)
- Smooth lighting for all nodes (_numberZero_)
- Added mesh generation delay (_numberZero_)
- PostgreSQL bugfix on blocks deletion (_nerzhul_)
- Windows integration enhancements (_???_)
- Wieldmesh natural orientation (_kilbith_)
- Memory leak fix on client disconnection (_nerzhul_)
- Various performance fixes (_???_)
- Fix Windows icon (_adrido_)
- Fix various minor memory leaks (_nerzhul?_)
- Recent LuaJIT fixes (_nerzhul_)
- Reduce network packet reading/writing memory usage (_???_)
- Binding tab now doesn't exit game when used (_sofar_)
- Light update for mapblocks (_juhdanad_)
- Client: reduce fake object reactions in client event queue (_nerzhul_)
- Crash fix when reading schematic calls from API in some cases (_nerzhul_)
- Limit sound volume when incorrect value was set into config (_???_)
- Fix Cursor lock problem when window is inactive (_krock_)
- Particles are now sent to client regarding distance (huge performance improvement on particle servers) (_paramat_)
- Really disable minimap when disabled in configuration (_nerzhul_)
- Fix a damage bug when falling from a very high height (_red-001_)
- Various documentation fixes (_???_)
- Expose singlenode mapgen to menu (_nerzhul_)
- Dropdown menu selection fix (_red-001_)
- Tooltip display unification between tooltip[] and list[] (_krock_)
- ServerActiveObjects are now removed when overtaking map limits (_nerzhul, paramat_)
- Chat console height can now be set by the player. (_Shara RedCat_)
- Additional option added to node highlighting drop-down. (_Shara RedCat_)

### Client Modding

- Introducing Client-Side modding (CSM for the initiated). You can now have local mods to read various client data and handle different client events. This new modding step is very secure, you don't have access to all standard Lua API, just a subset, to protect your computers. Mods should be installed in @user_path@/clientmods.
- You will also have access to Client side commands, starting with a dot.
- If you want to know more about CSM API, please look at [Client Side Modding documentation](https://github.com/luanti-org/luanti/blob/master/doc/client_lua_api.md)
- Added by nerzhul, red-001, bigfoot547, Dumbeldor and paly2.

### Server Modding

- Enable mod_security by default
- Add `minetest.player_exists()` (_rubenwardy_)
- Add player attributes backend. This permits modders to store misc player related data to core and retrieve it after player loading. The attribute save is done by core. (_nerzhul_)
- Add mod metadata API permitting mods to have a standard way to write their own data. We recommend you to use this instead of your custom files backend. (_nerzhul_)
- Add position & anchor attributes for formspecs (_adelcoding1_)
- Add `minetest.spawn_falling_node` call (_zaoqi_)
- Remove `core.cause_crash` Lua call (_nerzhul_)
- Add `on_flood` server Lua callback (_sofar_)
- Add clouds API (_bendeutsch_)
- Add private node meta to prevent some information from being leaked to client (example: chest contents) (_sfan5_)

### Other / Misc

- Redis backend authentication support (_sfan5_)
- PostgreSQL < 9.5 support (_???_)
- Code refactoring (Game, Environments) (_???_)
- Introduce clang-format on repository to check and reformat C++ code with our rules (~15% source code managed) (_nerzhul_)
- Move external libs outside of src/ to lib/ (_nerzhul_)
- Update jsoncpp embedded lib to last C++03 version (0.10.6) (_nerzhul_)
- Disable leveldb on Android (_Ekdohibs_)
- Implement daily gitlab package build for Debian/Ubuntu/Fedora (_nerzhul_)
- Translations updates (_Multiple people_)

### Minetest Game changes

## 0.4.14 → 0.4.15

0.4.15 was released on Dec 22, 2016.

No official changelog exists yet, however you can find an unofficial one here: [https://forum.luanti.org/viewtopic.php?p=243949#p243949](https://forum.luanti.org/viewtopic.php?p=243949#p243949)

## 0.4.13 → 0.4.14

0.4.14 was released on May 15, 2016.

### Features

- Add viewing range GUI setting (kilbith)
- New settings tab contain all possible settings (PilzAdam)
- WoW-style Autorun (Duane Robertson)
- Add server side ncurses terminal (est31)
- Add support for audio feedback if placing node failed (BlockMen)
- New 3D Mode: Pageflip (Dalai Felinto)
- Add Valleys mapgen (Duane Robertson)
- Add /admin command which says who the server admin is (Splizard)
- Add '/clearobjects quick' (kahrl)
- Minimap: show player markers (RealBadAngel)
- Add support for non-ASCII characters to chat console (ShadowNinja)
- Nodebox: Allow nodeboxes to "connect" (Auke Kok)
- Add option to disable entity selectionboxes (TriBlade9)
- Add option to change screenshot file format (kaeza)

### Bug fixes and Improvements

- Fix object position border checking (est31)
- Fix falling through nodes on world load (Christof Kaufmann)
- Add environment variable MINETEST_WORLD_PATH (SmallJoker)
- Fix crash regression when invsize formspec gets used (est31)
- Fix GUITable selection issues with trees (kahrl)
- Speed up and make more accurate relief mapping (RealBadAngel)
- Add option to give every object a nametag (BlockMen)
- Add support for limiting rotation of automatic face movement dir entities (sapier)
- Fix wield item glitch (RealBadAngel)
- Allow per-tiles culling (Auke Kok)
- Mapblock mesh: Eliminate meshgen lags (RealBadAngel)
- Fix jumping at node edge (gregorycu)
- Restore simple settings tab and add advanced settings as dialog (BlockMen)
- Mapblock mesh: Allow to use VBO (RealBadAngel)
- Update menu header image (Jean-Patrick Guerrero)
- Fix player dying on login (Ekdohibs)
- Mainmenu: Refactor tab UI code (Rui419)
- Fix hotbar placement on displays with low screen density (PilzAdam)
- Mainmenu: Unify favorite servers with main serverlist (kilbith)
- Builtin: Add basic_privs setting (rubenwardy)
- Optimize default settings for Android build (Maksim Gamarnik)
- Fix locked hardware buttons on Android (Maksim Gamarnik)
- Disallow stacking items with different meta (hunterdelyx1)

### Modding

- Add /emergeblocks command and core.emerge_area() Lua API (kwolekr)
- Add get_biome_id(biome_name) callback (Duane Robertson)
- Added minetest.wallmounted_to_dir (Fernando Carmona Varo)
- Allow setting chunk size in core.set_mapgen_params (kwolekr)
- ABMs: Make catch-up behavior optional (paramat)
- Decoration API: Add flag for placement on liquid surface (paramat)
- Add more ways to pass data to check_player_privs (Robert Zenz)
- Add option to disable back-face culling for models (BlockMen)
- Schematics: Add core.place_schematic_on_vmanip API (kwolekr)
- Add LuaSecureRandom (est31)
- Allow craft replacements to use groups (TeTpaAka)
- Add Lua interface to HTTPFetchRequest (Jeija)
- Implement AreaStore serialization (ShadowNinja)
- Add AreaStore custom ID API (ShadowNinja)
- Add an option to colorize to respect the destination alpha (Samuel Sieb)
- Lua_api.txt: Add warnings of l-system lighting bug (paramat)
- Add [resize texture modifier (SmallJoker)
- Make the inventory bar HUD take offset into account (rubenwardy)

### Mapgen

- Dungeongen: Remove floating frames (paramat)
- Blob ore: Fix partial blobs (paramat)
- Mapgen: Add 4D fractal mapgen (paramat)
- Mgfractal: Independent offset/slice params for mandelbrot and julia (paramat)
- Mapgen: Add global 'decorations' flag (paramat)
- Mgv5/v7/flat/fractal: More large pseudorandom caves (paramat)
- Mgfractal: Add 3D and 4D fractals (paramat)
- Mgvalleys: Add Dry Riverbeds (Duane Robertson)
- Mapgen: Spread both night and day light banks in spreadLight (kwolekr)
- Mgv7: Decrease cliff steepness (paramat)

### Other / Misc

- Clean up threading (ShadowNinja)
- Improve locale directory detection (est31)
- Add new ContentParamType2 "CPT2_DEGROTATE" (est31)
- Refactor logging (ShadowNinja)
- Improve rollback database indexing (cheapie)
- Mgfractal: Add documentation to conf.example and settingtypes (paramat)
- Add the player name to dropped items (Robert Zenz)
- Implement OSX Travis builds (Pavel Puchkin)
- Simplify custom games packaging (Pavel Puchkin)
- New timer design (Auke Kok)
- Add option to not send pre v25 init packet (est31)
- Clean up Strfnd (ShadowNinja)
- Add CONTRIBUTING.md (Craig Davison)
- Mainmenu: Standardize the menu button order and sizes (SmallJoker)
- Android: Increase player_stepheight for thicker snow nodebox (Maksim Gamarnik)

### Minetest Game changes

#### API changes

- A modding API was added to TNT, which allows mods to easily create explosion effects (red-001)
- A modding API was added to doors, which allows mods to create new doors that are feature-rich (sofar)
- A fence, wall, and fence gate API was added (sofar)
- A give_initial_items API was added (rubenwardy)

#### Interface changes

- Creative inventory now allows searching for nodes by name and description (kilbith)

#### Visual/Effect/Audio changes

- Several new textures were added by many different contributors (paramat, kilbith, sofar, kevdoy, Craig Davison, Wouters)
- Water texture alpha and water post effect color were changed (paramat)
- Steel door sounds were added (sofar)
- Flowers will wave when the waving plant shader is enabled (paramat)
- Doors are now made out of a single mesh and not two half nodes (sofar)

#### Mapgen/Landscape changes

- Grass can grow on sandy beaches and dunes (paramat)
- More flowers will grow in many biomes (paramat)
- Almost all biomes are now richer and more varied (paramat)
- Aspen trees were added to forests (sofar)
- Fallen logs were added (mgv5, mgv7), and mushrooms can grow on them (sofar)
- Dirt and sand blobs may appear in sandstone (paramat)

#### Gameplay changes

- Book interface was entirely rewritten to allow for proper pages and wrapping (kilbith, tenplus1, mt-modder)
- A metal sign was added, as well as a steel ladder (kilbith)
- mushroom spores were removed (sofar)
- a metal (locked) trapdoor was added (sofar)
- books can be copied in the craft grid (sofar)
- A new permanent flame node was added, as well as "flint and steel" (paramat, kilbith)
- Moss can grow on cobblestone if it gets wet (paramat)
- TNT was largely rewritten and has many new effects and behaviors (red-001, sofar)
- Stone walls were added for all cobble stone types (sofar)
- A simple Fence gate was added (sofar)
- The boat is now slightly faster (paramat)

## 0.4.12 → 0.4.13

0.4.13 was released on August 20, 2015.

### Features

- Add camera smoothing and cinematic mode (F8) (rubenwardy)
- Radius parameter for /deleteblocks here (SmallJoker)
- Save creative_mode and enable_damage setting for each world in world.mt (fz72)
- Configurable automatic texture scaling and filtering at load time. (Aaron Suen)
- Connect rails with connect_to_raillike and shorten the codes (SmallJoker)
- Clouds: Make cloud area radius settable in .conf (paramat)
- Added hour:minute format to time command (LeMagnesium)
- Add mod security (ShadowNinja)
- Add texture overriding (rubenwardy)
- Improved parallax mapping. Generate heightmaps on the fly. (RealBadAngel)
- Make attached objects visible in 3rd person view (est31)
- Remove textures vertical offset. Fix for area enabling parallax. (RealBadAngel)
- Add minimap feature (RealBadAngel, hmmmm, est31, paramat)
- Add new leaves style - simple (glasslike drawtype) (RealBadAngel)
- Add ability to specify coordinates for /spawnentity (Marcin)
- Add antialiasing UI setting (Mark Schreiber)
- Add wielded (and CAOs) shader (RealBadAngel)
- Add map limit config option (rubenwardy)

### Bug fixes and Improvements

- Add count based unload limit for mapblocks (est31)
- Kick players when shutting down server or on Lua crash (nerzhul)
- Fix relief mapping issues (RealBadAngel)
- Improve group-based connection between raillike nodes (BlockMen)
- Use skin font for usernames (fixes #2363) (BlockMen)
- Fix some memory leaks on packet sending. (nerzhul)
- Fix android build (nerzhul)
- Fix serialization of floating point numbers (ShadowNinja)
- Disallow object:remove() if the object is a player (Kahrl)
- Fix wrapDegrees family of functions (Zeno)
- Optimize MapBlockMesh related functions (gregorycu)
- Fix minor memory leak (Android) (Zeno)
- Fix occlusion (Miguel Almeida)
- ClientInterface::getClientIDs doesn't need a std::list. Use a std::vector for better performance (nerzhul)
- Fix some rendering glitches (BlockMen)
- Fix mapgen using uninitialized height map values (Zeno)
- Fix Android text bug (no text displaying) (Zeno)
- Improve Clouds::render mathematics (nerzhul)
- For usages of assert() that are meant to persist in Release builds (when NDEBUG is defined), replace those usages with persistent alternatives (Zeno)
- Fix RUN_IN_PLACE broken due to invalid usage of assert (sapier)
- Respect game mapgen flags and save world noise params (ngosang)
- Don't use luaL_checkstring to read node names, it's only for arguments (ShadowNinja)
- Heightmaps: Fix uninitialized values in mgv5/mgv6. findGroundLevel: Return -MAP_GENERATION_LIMIT if surface not found (paramat)
- Make the dummy backend only look up blocks once (ShadowNinja)
- Fix uninitialized data when creating TOSERVER_INIT packet (nerzhul)
- Fix memory leak pointed by issue #2439. Also change bzero to memset. bzero doesn't work on windows (nerzhul)
- Stop formspecs closing with double-click in empty area (Zeno)
- Ensure that heightmap is initialized before use (Zeno)
- lua_api/l_mapgen: Fix overlapping areas of minetest.generate_ores/decorations (paramat)
- Mgv6: Fix uninitialized heightmap used by cavegen (paramat)
- Disable double-click -> ESC translation for main menu (Zeno)
- If player is dead, permit it to respawn, even if damages are not enabled (nerzhul)
- Android: Fix auto-entry of server address and port in mainmenu (est31)
- Fix various damage related bugs (client-side) (Zeno)
- Minor bug fix (lag between damage flash and hearts updating) (Zeno)
- Fix game minetest.conf default settings (est31)
- Optimize minetest.get\_(all)\_craft_recipe(s) (gregorycu)
- Fix composite textures with texture_min_size (Aaron Suen)
- Protect Player::hud from concurrent modifications (nerzhul)
- Fix minetest.get_craft_recipe function (est31)
- Fix set_bits (kwolekr)
- Fix usage of destroyed mutex (kwolekr)
- Fix crash caused by null texture in GUI formspec/HUD. (Aaron Suen)
- Fix players spawned at (0,0,0) in some rare cases instead of static_spawnpoint (nerzhul)
- Crafting speedup (est31)
- Fix uninitialized variables in ConnectionEvent (nerzhul)
- Fix a rare crash case un SendPlayerHP (nerzhul)
- Schematics: Fix core.schematic_create() (kwolekr)
- fix infinite spawners (obneq)
- Disable connection timeout for singleplayer and server tabs (est31)
- Fix mod store rating (ShadowNinja)
- Fix sign-compare compiler warnings in mg_ore.cpp (ShadowNinja)
- Fix player pitch and yaw not being set properly (Kevin Ott)
- Fix fast leaves with texture_clean_transparent enabled. (Aaron Suen)
- Fix minetest.clear\_\* creating new LOCAL table instead of clearing the existing one. (Tomas Brod)
- Noise: Fix PcgRandom::randNormalDist() when range contains negative numbers (kwolekr)
- Add a check for animation when getting an extruded mesh (Kevin Ott)
- Stop NetworkPacket methods from producing bloated packets (Jay Arndt)
- Replace Wieldmesh::setItem assertion that could be triggered by the server with an error (kwolekr)
- Ensure that Map::findNodesWithMetadata() reports nodes strictly within the node-granular area (kwolekr)
- Fix typo in WieldMesh::setItem() (kwolekr)
- Don't crash if an item gets dropped into unloaded space (tenplus1)
- ANDROID: Do not limit situations where fast is enabled (Zeno)
- Fix current mod name change missed during rebase (ShadowNinja)
- Noise: Fix interpolation at negative coordinates (kwolekr)
- (Android) Only simulate holding down fast key if fast_move is toggled to true (Zeno)
- dofile error reporting for syntax errors (est31)
- Don't crash when saplings try to grow on unknown nodes (y.st, ShadowNinja)
- Remove unnecessary space for tab completion (Nathaniel Olsen)
- Fix some issues with animations, and allow non-looped animations to be defined (MirceaKitsune)
- Fix bug when craft input isn't replaced (TeTpaAka)
- Fix string conversion error message (est31)
- Fix bugs in mainmenu (kilbith, jp)
- Fix single click world select (est31)
- Shaders fixes and cleanup relief mapping code. (RealBadAngel)
- Fix missing check for 0 in craft replacements (TeTpaAka)
- Craftdef: Use numbers instead of iterators (est31)
- Fix attempt to start a world when no world is selected/created (kilbith)
- Fix endless loop since grandparent commit (est31)
- Fix damage flash when damage disabled (kwolekr)
- Add more robust error checking to deSerialize\*String routines (kwolekr)
- Fix minetest.get\_(all)\_craft_recipe(s) regression (est31)
- Fix FSAA dropdown option reset after changing another dropdown option (kilbith)
- Fix MSVC number conversion warning (SmallJoker)
- Fix srp.cpp:815 leak (est31)
- Fixed minimap memory leak (Břetislav Štec)
- Android: Fix minor makefile bugs (est31)
- src/network/connection.h: Fix race condition (Břetislav Štec)
- src/environment.cpp: Fix NULL pointer dereference (Břetislav Štec)
- Improve accuracy and safety of float serialization (kwolekr)
- src/client.cpp: Fix mapper memory leak (Břetislav Štec)
- src/wieldmesh.cpp: Fix mesh extrusion memory leak (Břetislav Štec)
- Android: fix sound issue, and gitignore (est31)
- src/client/tile.cpp: Fix reference counting (Břetislav Štec)
- Fix "bouncy" blocks (Miner59)
- src/util/numeric.{cpp,h}: Fix FacePositionCache data race (Břetislav Štec)
- Fix tiling issues for PLANTLIKE and FIRELIKE with FSAA (RealBadAngel)
- connection: Make assertions non-fatal for received data (kwolekr)
- Fix critical vulnerabilities and bugs with NetworkPacket (kwolekr)
- Fix BufferedPacket race condition (fixes #2983) (kwolekr)
- Fix detection of sneaking node This fixes bug 1551 (gregorycu)
- Fix camera updates being toggled by N key in release mode (#2762) (Kahrl)
- Fix segfaults caused by the Environment not being initialized yet (rubenwardy)
- Display Lua memory usage at the time of Out-of-Memory error (kwolekr)
- Make NetworkPacket respect serialized string size limits (kwolekr)
- Fix intlGUIEditBox leak and uninitialized value in Mapper (reported by valgrind) (Kahrl)
- Fix Lua PcgRandom (est31)
- Fix segfault caused by a8e238ed06ee8285ed4459e9deda3117419837f6 (Perttu Ahola)
- Fix sneaking (fixes #665 and #3045) (BlockMen)
- Rollback: Fail on bad precondition instead of causing assertion error (kwolekr)
- Fix inventory replace bug (est31)
- Fix indianred and indigo of color-string (Rui)
- Optimizations (multiple)

### Modding

- Add mod.conf file support - allows mods to specify a mod name for now (kaeza)
- Add find_nodes_in_area_under_air (nerzhul, Zeno)
- Add core.register_schematic() and cache schematics on use (kwolekr)
- Schematics: Reorganize (de)serialization and add Lua serialization API (kwolekr)
- Add minetest.global_exists() (ShadowNinja)
- Fix pathfinder to produce more useful paths (obneq)
- Add core.find_nodes_with_meta() script API (kwolekr)
- Schematics: Add per-node force placement option (kwolekr)
- is_player() is no player-only function (est31)
- Add code to support raillike group names (Novatux)
- Add get and set functions for the nametag color (TeTpaAka)
- Add minetest.register_on_punchplayer (Brandon)
- Schematics: Fix probability values for .mts version 1 (kwolekr)
- Add core.mkdir (ShadowNinja)
- Add core.request_insecure_environment() (ShadowNinja)
- Add core.get_dir_list (ShadowNinja)
- SAPI: Accept either ARGB8 table or ColorString to specify colors (kwolekr)
- Add some missing getter functions to the lua API (TeTpaAka)
- Decrease minetest.after globalstep lag (HybridDog)
- Add return list of individual counts to find_node_in_area (TeTpaAka)
- Add minetest.register_on_player_hpchange (TeTpaAka)
- Add list-rings (est31)
- Add Lua errors to error dialog (rubenwardy)
- Biome API decorations: 'spawnby' searches a 3D neighbourhood (paramat)
- Make acc and vel deprecated in add_particle and search for acceleration and velocity instead (TeTpaAka)
- Added get_player_velocity() method. Fixes #1176 (Elia Argentieri)
- Allow random menu images for games (sfan5)
- Document game main menu image system (est31)
- Add AreaStore data structure (est31)
- Actually document what minetest.is_protected should do (est31)
- SAPI: Track last executed mod and include in error messages (kwolekr)

### Mapgen

- Mgv5: Remove blobgen. Remove crumble and wetness noises (paramat)
- Biome API: Re-calculate biome at every surface in a mapchunk column (paramat)
- Mgv6: Add heightmap. Do not make large caves that are entirely above ground (paramat)
- Cavegen, mgv5: Cleanup code (paramat)
- Fix memory leak in MapgenV6 (Zeno)
- Biome API: Enable decorations
- Mgv5/mgv7: Add desert temples if desert stone detected in mapchunk (paramat)
- mg_decoration: Raise highest allowed deco top to max edge of voxelmanip (paramat)placed on water (paramat)
- Mgv6: Remove addDirtGravelBlobs, replaced by blob ore in Minetest Game (paramat)
- Mgv5/mgv7: Sprinkle dust from full_node_max.Y if chunk above is generated (paramat)
- Mgv7: 1 up , 1 down overgeneration for chunk border continuity (paramat)
- lua_api/l_mapgen: generate_ores/decorations: make p1, p2 optional (paramat)
- ObjDefManager, Mapgen SAPI: Huge refactoring (kwolekr)
- Treegen: Add pine tree. Force place trunks (paramat)
- Mgv6: Add optional snow biomes (paramat)
- Mgv6: Fix taiga, allow pine tree spawning on snowblocks (paramat)
- Mgv5/v7: Add check for water for deciding biome node stability (paramat)
- Mgv5: Fix above/below ground spawn when water level is altered (paramat)
- Biome API: Add biome-specific river water (paramat)
- Noise: Correct noise objects created with invalid dimensions (kwolekr)
- Ore: Add biomes parameter (kwolekr)
- Noise: Add noise unittests (kwolekr)
- Mapgen v5/6/7: Cleanup node resolver and aliases (paramat)
- Noise: Make buffer size parameters unsigned (kwolekr)
- Mapgen v5/v7: Detect sandstone, enable sandstone brick dungeons (paramat)
- SAPI/Noise: Add PerlinNoiseMap:getMapSlice() function (kwolekr)
- Mgv5/v7: Fix generateBiomes biome recalculation logic Biomegen down to y = -192 for mgv5 deep oceans. Improve code (paramat)
- Biome API, mgv7: Increase heat/humidity spreads. Improve mgv7 noise parameters (paramat)
- Mgv6: Enable snow biomes by default. Double biome noise spread. 3 octaves, 0.5 persistence for humidity (paramat)
- Mgv5/mgv7: Trigger biome recalculation at underwater surfaces (paramat)
- Minimal: Edit mapgen aliases. Use blob ore for clay, update other ores. Update simple biomes. Cleanup code (paramat)
- Minimal: Add snow biome and jungle leaves nodes. Add mapgen aliases (paramat)
- Biome API: Enable biome generation to lower world limit (paramat)
- Mgv6: Don't create air gap in tundra at y = 48 in custom high terrain (paramat)
- Biome API: Add noise defined biome blend (paramat)
- Mapgen objects: Enable heatmap and humidmap for all biome api mapgens (paramat)
- Mgv7: Edit noise parameters. Fewer octaves, larger spreads. (paramat)
- Mgv5/mgv7 caves: Remove sand found in underground tunnels (paramat)
- Biome API: Increase heat and humidity noise spreads to 1000 (paramat)
- Cavegen: Cleanup code. Define constant for MGV7_LAVA_DEPTH (paramat)
- Mgv7: Lower base of mountain generation to -112 and define constant (paramat)
- Mgv7: Auto-set lowest mountain generation level (paramat)
- Cavegen: Mgv6: No small caves entirely above ground (paramat)
- Mgv7: Use density noise + density gradient for mountain terrain (paramat)
- Treegen: Rename pine tree mapgen alias (paramat)
- Biome API: Make fallback biome stone and water, disable filler (paramat)
- Cavegen V6: Make all caves consistent with 0.4.12 stable (paramat)

### Other / Misc

- Start adding utf-8 support (est31, Ilya Zhuravlev)
- Unit tests must be done at integration process. (nerzhul)
- Improve FindIrrlicht.cmake module (Markus Koschany)
- Rename --do-unittests to --run-unittests as @Zeno- and @sfan5 requested (nerzhul)
- Clean up database API and save the local map on an interval (ShadowNinja)
- Don't start a server for map migration (ShadowNinja)
- Dungeongen: Optionally set ignore to be untouchable to disable floating dungeons (paramat)
- Finer progress bar updates when initializing nodes (est31)
- Minor cleanup: game.cpp (Zeno)
- Add support for the PCG32 PRNG algo (and associated script APIs) (kwolekr)
- Change filename of screenshots to something more human readable (Zeno)
- Clean scaling pre-filter for formspec/HUD. (Aaron Suen)
- Remove errorstream logging on password change (est31)
- Add reason to kicked log message and use present tense (est31)
- RotateAlongYAxis: For facedir case, return if param2 >= 4 (paramat)
- Change lower limit of display_gamma to 1.0 (linear light) (Zeno)
- More reliable serverlist behavior (HybridDog)
- Close keybind settings menu with esc (est31)
- Disable mesh cache by default (est31)
- Set server_announce to world.mt and respect modes when changing game (Sokomine)
- Use minetest logging facilities for irrlicht log output (ShadowNinja)
- Display an access denied message when client detects a server timeout (Kahrl)
- Change texture pack description file name (ExcaliburZero)
- Refactor particle code to remove the while loops (TeTpaAka)
- MoveItemSomewhere double bugfix (est31)
- Remove profiler.h include where it's not needed. Remove some unreachable and very old code (nerzhul)
- Ask auth handler to create auth when a default password is set (est31)
- Optional reconnect functionality (est31)
- Fix documentation of dedicated_server_loop (est31)
- Remove drivers dropdown in the settings tab (kilbith)
- Cleanup server addparticle(spawner) by merge two identical functions. (nerzhul)
- Precalculate mapblock relative size. This permit to remove many s16 calculs on runtime (nerzhul)
- Android: Add githash header to spare rebuilds after new commits (est31)
- Prepend "Lua: " before lua exceptions src/server.cpp src/emerge.cpp (Břetislav Štec)
- Improve Script CPP API diagnostics (kwolekr)
- Initialize random for verification key generation too (est31)
- game.cpp: Update cached settings (est31)
- SAPI: Disable unlockable time profiling (kwolekr)
- Client: disable mmdb modstore (est31)

## 0.4.11 → 0.4.12

0.4.12 was released on February 18, 2015.

### New features

- Add player direction to debug text (_yamanq_)
- Reorganized client and server tabs (_kilbith_)
- Implemented DPI automatic detection on X11 (_sapier_)

### Map generation

**v5:**

- Caves check for biome nodes, only excavate stone under water level (_paramat_)
- Unease caves noises, use 0.3.x parameters (_paramat_)
- Blobgen after cavegen (_paramat_)
- Biomegen: remove “is replaceable content” bool (_paramat_)

### Tweaks

- Increased step height on Android (_sapier_)
- Increased default `font_size` (_BlockMen_)
- Improved minetest.desktop, added German and French text to minetest.desktop (_nerzhul_)
- More consistent progress bar (_sapier_)

### Bug fixes

- Fixed `font_size` under Windows (_BlockMen_)
- Ignored old entities from 0.3 (_Novatux_)
- Fixed FTBFS on GNU/Hurd platform (_apoleon_)
- Modified Y positioning of health/breath statbars to prevent overlapping with hotbar (_kwolekr_)
- Fixed memory leaks related to gettext (_ShadowNinja_)
- Give full breath after death (_SmallJoker_)
- Fix `NDT_GLASSLIKE` normals (_kahrl_)
- Water flowing fixes (_gregorycu_)
- Compiler tweaks and warning fixes (_ShadowNinja_, _kwolekr_)
- Fix imprecise serialization of large numbers (_ShadowNinja_)
- Fix performance regression (_Zeno-_)
- Fix getCraftRecipe returning wrong recipes (_sapier_)
- Fix unused (and so, broken) `enable_rollback_recording.` (_nerzhul_)
- Fix .zip extraction (mod store) (_ngosang_)
- Fix translation memory leak (_ShadowNinja_)
- Fix F7 crash (_nerzhul_)
- Fixes to default screenshots in mainmenu (_Rui914_)
- Fix `map_seed` not changed when creating a new world after login to another (_fz72_)
- Add modname convention checking, fixes issues with mod enabling (_est31_, _Novatux_)
- Fix problems related to still receiving damage after dying (_SmallJoker_, _gregorycu_)
- Add `vein` and `blob` ore type (_kwolekr_)
- Change assignment to global in a function to warning (_rubenwardy_)

### Vanilla game changes (minetest_game)

#### Gameplay

- Mossy cobblestone can now be smelted to stone (_MT-Modder_)
- Added straw, crafted with 9 wheat (_kilbith_)
- Added obsidian and obsidian brick stairs and slabs (_CraigyDavi_)

#### Visuals

- Many new textures renewed (_kilbith_)
- Changed furnace fire icons (_Kalabasa_)
- Added fancy inventory for bones (_CraigyDavi_)

### Master server (server list)

- Announce MIN/MAX protocol version to server list (_est31_)

## 0.4.10 → 0.4.11

0.4.11 was released on December 24, 2014.

### New features

**Big gameplay changes**

- Added mapgen v5 (_paramat_)
- Added `enable_build_where_you_stand` option (_Sokomine_)

**Smaller gameplay tweaks**

- Added inventory right click drag and drop (_sruz25, Zeno_)
- Remove `buildable_to` nodes without dropping item when replaced by a falling node (_Casimir_)

**Visual changes**

- Reduced time of red screen when damaged (_SmallJoker_)
- Added video driver selection to settings menu (_sapier, webdesigner97_)
- Removed alpha channel from screenshots (_BlockMen_)
- Added node highlighting (_RealBadAngel_)
- Added configurable selection box width. Min width = 1, max = 5 (_TriBlade9_)
- Changed default halo.png for better visibility (_RealBadAngel_)
- Added in-game key change menu (_Mushiden_)
- Improved lighting of the wielded item (_kahrl, RealBadAngel_)
- Increased step smoothing to fit 1:1 stairs (_Calinou_)
- Scale form elements consistently using new font engine by sapier (_Zefram_)
- Made dropped items larger and rotate faster (_Calinou_)
- Increase third person view distance (_Calinou_)
- Made directional fog colors respect tone map (_Taoki_)
- Display serverlist flags as icons (_kahrl, kilbith, VanessaE et al._)

**Build system changes**

- Added `ZLIBWAPI_DLL` and `LEVELDB_DLL` CMake options (_sfan5_)
- Removed legacy `MINGWM10_DLL` CMake option (_sfan5_)
- Changed build directory for build bots to `_build` to prevent removal of Android build files (_sfan5_)
- Updated 32-bit build bot (OpenAL updated, zlib updated) (_sfan5_)
- Added -win64 suffix to build bots for 64-bit Windows builds (_sfan5_)
- Updated the cURL the buildbot uses to 7.38.0 (_sfan5_)
- Added Android Makefile support for builds without LevelDB (_sapier_)
- Improved Travis CI configuration (_Mikaela Suomalainen_)
- Added gettext to Travis build (_ShadowNinja_)
- Build for win32 & win64 on Travis too (_sfan5_)
- Update MinGW toolchain downloads used by Travis (_sfan5_)
- Fixed various build issues on Windows/MSVC (_SmallJoker_), Android (_sapier, Kodexky_), Mac OS X (_Pavel Puchkin_)

**Logistical changes**

- Don't unload blocks if save failed (_kwolekr_)
- Don't copy back already generated blocks on map generation (_kwolekr_)
- Moved MapBlock (de)serializing code out of Database class (_sfan5_)
- Don't include `cmake_config_githash.h` into files that don't need it (_sfan5_)
- Moved #includes from version.h to version.cpp (_kahrl_)
- Improved timeout calculation when packets are lost (_sapier_)
- Refactored a section of ban.cpp (_Selat_)
- Allowed use all 6 faces for special tiles (_RealBadAngel_)
- Saved previously generated blocks on Mapgen blitback (_kwolekr_)
- Refactored Settings (_ShadowNinja, Zeno, kwolekr_)
- Added setting groups (used for NoiseParams) and multiline entries (_kwolekr_)
- Added NodeResolver and cleaned up node name -> content ID resolution system (_kwolekr_)
- Added support for eased 3d noise (_kwolekr_)
- Added notice when only minimal is installed (_rubenwardy_)
- Split up mapgen.cpp (_kwolekr_)
- Refactored `the_game` (_Zeno_)
- Added Generator Element Management framework (_kwolekr_)
- Cleaned up rollback (_ShadowNinja_)
- Refactored main.cpp (_Zeno_)
- Rewrote generate notification mechanism (_kwolekr_)
- Rewrote fs::GetDirListing(file), fixing a potential buffer overflow (_kahrl_)

**Other changes**

- Removed math mapgen (_proller_)
- Removed indev mapgen (_proller_)
- Removed proller from credits (_proller_)
- Updated default control documentation (_BlockMen_)
- Made lighting CPU-only by removing finalColorBlend implementation from shaders (_RealBadAngel_)
- Added `/dummyball <count>` command to the minimal game (_kahrl_)
- Made the LuaJIT exception wrapper handle more exceptions (_kahrl_)
- Added missing doc for `minetest.get_us_time()` (_sapier_)
- Made HTTPFetch use the configured `bind_address` (_ShadowNinja_)
- Made config compatible with C++ 2011 (_donat_b_)
- Added a .mailmap file (_Stefan Beller_)
- Search for games using `$MINETEST_SUBGAME_PATH` (_David Thompson_)
- Added Indonesian language (_srifqi_)
- Updated translations (_kilbith, ShadowNinja_)
- Replaced setting `unlimited_player_transfer_distance` with `player_transfer_distance` (_SmallJoker_)
- Added `last_login` field to auth.txt (_Ryan Newell_)
- Added tooltips to main menu games button bar (_Wuzzy_)
- Added option 'eased' to NoiseParams (_SmallJoker_)
- Added (optional) client-side saving of server map to disk (_sfan_)
- Added name of node 'pointed at' to debug (_rubenwardy, Zeno_)
- Added space between client names in status text (_Muhammad Rifqi Priyo Susanto_)
- Disabled loading .mtl files (_RealBadAngel_)
- Made biome heat and humidity noise parameters user-configurable (_kwolekr_)
- Added paste command (Ctrl-V) in chat console (_kahrl_)
- Responsive tooltip offset for Android (_Kodexky_)
- Allowed footstep sounds to play for liquid and ladder nodes (_Taoki_)
- Added basic support for generating API documentation using Doxygen (_Jürgen Doser_)
- Set `WM_CLASS` window hint for Xorg (_kwolekr_)

### Performance

- Disabled `preload_item_visuals` by default (_ShadowNinja_)
- Sped up `mapblock_mesh` (_RealBadAngel, Zeno_)
- Implemented caching of facedir rotated meshes (controlled by `enable_mesh_cache` setting) (_RealBadAngel_)
- Removed most exceptions from getNode() (and variants) (_Zeno_)
- Sped up removing a node (less block mesh updates) (_RealBadAngel_)
- Reduced number of extrusion meshes to (usually) 5 instead of one per item (_kahrl_)
- Improved VoxelArea variable locality (_Wouters Dorian_)
- Optimized functions from CNodeDefManager and VoxelManipulator (_Zeno_)
- Optimized serialization, for example by using machine native byte swapping if available (_Rafael Reilova_)
- Optimized main client loop (_Zeno_)
- Optimized noise implementations (_kwolekr_)
- Optimized getLight() by 2x (_Zeno_)
- Stopped liquid queue from eating up more and more RAM; also `liquid_loop_max` now defaults to 100000 (_Zeno, celeron55_)
- Changed TileSpec::frames to be std::vector not std::map (_unknown, Zeno_)

### Bug fixes

- Fixed face shading issues (_RealBadAngel_)
- Fixed crash reported here: [https://forum.luanti.org/viewtopic.php?f=6&t=9726](https://forum.luanti.org/viewtopic.php?f=6&t=9726) (_Novatux_)
- Fixed flipped textures for drawtype "glasslike" (_sapier_)
- Fixed indexing error in timer processing (_Zefram_)
- Made `tooltip_show_delay=0` work (_Zefram_)
- Fixed error handling on inconsistent client ready message (_sapier_)
- Fixed texture hack in fences (_RealBadAngel_)
- Fixed texture glitches for plants with visual scale > 1.0 (jungle grass) (_RealBadAngel_)
- Fixed makeCuboid to apply rotations to all faces when 1 tile is given (_RealBadAngel_)
- Fixed display of interior of `glasslike_framed` node when its not defined (_RealBadAngel_)
- Fixed LuaVoxelManipulator memory leak (_Zeno-_)
- Fixed seeds corrupting world creation menu formspec (_ShadowNinja_)
- Made face shading correct for all possible lighting modes (_RealBadAngel_)
- Fixed liquid sources and flowing surfaces having different brightness (_RealBadAngel_)
- Fixed main menu game initialization (_BlockMen_)
- Made safeWriteToFile() remove empty file if there is an error (_Selat_)
- Don't call player events without having player to do a event for (_sapier_)
- Fixed "ghost" blocks if block update is "on wire" while player digs nodes (_sapier_)
- Added player name length checks (_sapier_)
- Fixed chat messages capturing mouse interactions for menu/formspecs (_sapier_)
- Fixed segmentation fault if popping from empty stack (L-system trees) (_Zeno_)
- Fixed interlaced 3D mode second image being flipped when compiling with Irrlicht 1.8+ (_sapier_)
- Fixed only one texture being updated on window resize, breaking side-by-side and top-bottom 3D modes (_sapier_)
- Fixed access to invalid data when receiving empty packets (_sapier_)
- Fixed some locking bugs (_kahrl, ShadowNinja_)
- Use round if falling node is misplaced (_SmallJoker_)
- Fixed and simplified player modification checks (_ShadowNinja_)
- Fixed unit tests failing if IPv6 not available (_Zeno_)
- Fixed wield mesh getting clipped by camera far value (_kahrl_)
- Fixed raillike rendering bug on Android (_Kodexky_)
- Don't corrupt stepheight when setting other properties (_Ciaran Gultnieks_)
- Fixed mouse events getting passed from a table's scrollbar to its parent (_kahrl_)
- Fixed `minetest.place_schematic()` when defined by a Lua table (_kwolekr_)
- Ignore .name directories and files in main menu (_SmallJoker_)
- Fixed some typos (_sapier, rubenwardy, William Teder, ShadowNinja, kahrl, Zeno_)
- New drawtypes: mesh (_RealBadAngel_), firelike (_TriBlade9_), `glasslike_framed_optional` (_BlockMen_)
- New texture modifiers: ^[mask (_sfan5_), ^[colorize (_BlockMen_)
- New formspec element: scrollbar (_sapier_)
- Allowed non-integer sizes for `item_image[]` (_Zefram_)
- Added texture grouping via ( ... ) (_sfan5_)
- Added mod profiling support (_sapier_)
- Added compression API (_ShadowNinja_)
- Added update of the Mapgen VoxelManipulator on buffer invalidation (_kwolekr_)
- Added LuaVoxelManip methods: `get_node_at()` and `set_node_at()` (_kwolekr_)
- Simplified and optimized schematic replacements (_ShadowNinja_)
- Made dump's output prettier (_ShadowNinja_)
- Added `collision_box` node property (_RealBadAngel_)
- Added warning when creating a global variable (unless it has the same name as the current mod) (_ShadowNinja_)
- Added `minetest.copy_table(table)`, `vector.apply(v, func)`, and `math.sign(x, tolerance)` (_SmallJoker_)
- Improved documentation for `remove_item`, `string_to_pos`, `dig_node`, `on_step`, `get_meta` (_Ciaran Gultnieks_)
- Added `minetest.clear_registered_biomes()` (_kwolekr_)
- Added new noise parameters: flags and lacunarity (_kwolekr_)
- Added support for NoiseParams in `minetest.get_perlin()` (_kwolekr_)
- Exposed mapgen chunk size in `on_mapgen_init` callbacks (_kwolekr_)

### Vanilla game changes (minetest_game)

#### Gameplay

- Made opened trapdoor climbable (_Zefram_)
- Made doors form double-doors with other doors of any kind (_Zefram_)
- Added filled buckets to creative inventory (_Zefram_)
- Enabled dungeon generation by default (_Amaz1_)
- Improved handling of boats (_paramat_)
- Added pine trees, with needles, tree, wood and saplings (_paramat, PilzAdam_)
- Made player-placed leaves not decay (_PilzAdam_)
- Reworked infotext of furnace (_PilzAdam_)
- Added Obsidian Bricks (_HybridDog_)
- Changed controls of screwdriver (_tenplus1, PilzAdam_)

#### Visuals

- Changed ingot textures (_Nore_)
- Made TNT smoke round (_ShadowNinja_)
- Made fire use new fire drawtype (_BlockMen_)
- Added new textures for cobble and furnace (_Neuromancer56, BlockMen_)
- Added new textures for grass (_BlockMen, Philipbenr_)
- Made glass use new, optional framed drawtype (_BlockMen_)
- Added new textures for apples, chests, dirt, desert stone bricks, HP bar, snowballs (_BlockMen_)
- Added new textures for ores, vessels, grass (plant), leaves and ladders (_kilbith_)
- Added new textures for flowers (_RHRhino_)
- Added new textures for doors (_Amaz1_)
- Changed soil textures to use an overlay over `default_dirt.png` (_PilzAdam_)
- Added new textures for soil (_PilzAdam_)
- Added a bit white to crack texture (_ShadowNinja_)
- Made signs a 3D box, instead of just a 2D plane (_Calinou_)

#### Bug fixes

- Fixed crafting recipe for iron bars (_BlockMen_)
- Added protection support to TNT (_BlockMen_)
- Retain sign text when editing is aborted via Esc (_Zefram_)
- Fixed Desert Sand Soil dropping itself (_Amaz1_)
- Consistently use group:stick in tool recipes (_Zefram_)
- Fixed boats flying upwards (_paramat_)
- Fixed hoes wearing out in creative mode (_BlockMen_)
- Fixed brown dye being in yellow color group (_BlockMen_)
- Fixed player staying attached when removing boat (_BlockMen_)
- Removed “leaked” global variables (_PenguinDad, kaeza, CraigyDavi, PilzAdam_)
- Fixed various bugs with fire sounds (_PilzAdam_)
- Fixed possible stacking of books in bookshelf (_MT-Modder_)
- Fixed soil drying out if nearby water is unloaded (_PilzAdam_)
- Fixed rotating of locked doors to bypass them (_PilzAdam_)
- Fixed screwdriver overriding bits in param2 that are not used for rotation (_PilzAdam_)

#### Miscellaneous

- Added `enable_tnt` setting (_ShadowNinja, Yepoleb_)
- Optimized TNT explosion (_ShadowNinja_)
- Added option for custom opening and closing sound for doors (_Jat15, Zefram_)
- Removed generation of flowers, papyrus, cactus and grass (plant) generation from other map generators than v6 (_paramat_)
- Removed ore definitions for indev mapgen (_paramat_)
- Added all saplings to group sapling (_PilzAdam_)
- Allowed the group book to be placed in bookshelf (_PilzAdam_)
- Added a minetest.conf.example with all settings from `minetest_game`, that can be changed in minetest.conf (_PilzAdam_)
- Removed remains of weather and finite liquids (_PilzAdam_)
- Restructured and moved furnace code to furnace.lua (_PilzAdam_)

### Master server (server list)

- Announce `mg_name` from `map_meta.txt` instead of minetest.conf (_kahrl_)

## 0.4.9 → 0.4.10

0.4.10 was released on July 6, 2014.

### New features

**Big gameplay changes**

- Added third person view (_BlockMen_)
- Removed finite liquid and weather (_proller_)

**Smaller gameplay tweaks**

- Create bones only when the player's inventory is not empty & remove the bones when emptied (_arsdragonfly_)
- Made pause menu actually pause singleplayer game and use lower maximum FPS in it (_celeron55_)
- Prevented placing node when player would be inside new node (_BlockMen_)
- Drop an item instead a stack while sneaking (_Lord89James_)
- Added support for exiting formspecs by double-clicking outside (_sapier_)

**Logistic changes**

- Made build prefer pkg-config for freetype2 detection (_hasufell_)
- Added function to deregister a profiler from profiler list (_sapier_)
- Made MutexQueue use jsemaphore for signaling (_sapier_)
- Added maximum recursion depth to `read_json_value` (_ShadowNinja_)
- Made default User-agent follow RFC 2616 (_ShadowNinja_)
- Include system info in the HTTP user agent on Windows (_sfan5_)
- Added proper client initialization (_sapier_)
- Settings: Add no-exception variants of each get method (_kwolekr_)
- Huge overhaul of the entire MapgenParams system (_kwolekr_)
- ServerEnvironment: Remove direct dependency on EmergeManager (_kwolekr_)
- Replace pause, message, and death menus by formspec ones (_sapier_)
- Pass arguments by reference, reducing data copies (_Selat_)
- Cleaned up client init states by bumping protocol version (_sapier_)
- Added support for named threads on Linux, BSD, and Windows (MSVC-only) (_sapier, ShadowNinja_)
- Inferred `ipv6_server` from `bind_address;` fixed client connect to `IN(6)ADDR_ANY` (_kahrl_)
- Fixed all warnings reported by clang (_sfan5_)
- Removed locks that aren't absolutely required from JThread (_sapier_)
- Use `narrow_to_wide` in gettext instead of operating system dependent conversion function (_sapier_)
- Organized builtin into subdirectories (_ShadowNinja_)
- Switched to "core" namespace internally (_ShadowNinja_)
- Mapped Irrlicht log level to Minetest's and allowed writing Irrlicht logs to debug file (_RelaBadAngel_)
- Made print() NUL-safe (_ShadowNinja_)
- Added formspec toolkit and re-factored main-menu to use it (_sapier_)
- Reworked dumping functions (_ShadowNinja_)
- Improved performance by removing some temporary objects (_sapier_)
- Added an AppData file (_David Gumberg_)
- United node shaders and improved shader performance (_RealBadAngel_)
- Made players only stay loaded while they're connected (_ShadowNinja_)
- Added formspec API versioning (_sapier_)
- Added support for multipart/form-data to HTTPFetch (_ShadowNinja_)
- Improved error reporting of LevelDB backend (_sfan5_)

**Visual changes**

- Added waypoint HUD element (_RealBadAngel_)
- Added on-the-fly normal map generation (_RealBadAngel_)
- Made sun and moon texture-able (_RealBadAngel_)
- Made formspec text-area word-wrap (_RealBadAngel_)
- Added support for DPI based HUD scaling (_sapier_)
- Made debug text adjust it's border to the screen size (_ShadowNinja_)
- Added download rate to non-HTTP media progress bar (_sapier_)
- Added support for interlaced-polarized, top-bottom, and side-by-side 3D screens (_sapier_)
- Made pause menu hide before "Shutting down..." message is drawn (_sapier_)
- Sorted commands and privileges alphabetically in '/help' (_kaeza_)
- Improved face shading with and without shaders (_RealBadAngel_)
- Added support for scalable font and GUI elements (_sapier_)
- Made GUITable mouse wheel scrolling faster (_kahrl_)

**Other changes**

- Added the option to bind to a specific address (_ShadowNinja_)
- Made flag strings clear specified flag with 'no' prefix (_kwolekr_)
- Added check to avoid usage of broken LuaJIT < 2.0.0-beta-8 (_sapier_)
- Lots of new and updated translations (_many contributors_)
- Improved performance of ABMs by only calculating object counts once (_CiaranG_)
- Improved win32 file version information (_sapier_)
- Documented CMake options in README (_sfan5_)
- Corrected misleading detached inventory error message (_CiaranG_)
- Added more informative error messages for inventory and item method errors (_ShadowNinja_)
- Added redis database backend (_sfan5_)
- Moved the old stuff to doc (_BlockMen_)
- Removed dependency on marshal and many other async changes (_ShadowNinja_)
- Made item entity stacks merge on the ground and add TTL to item entities (_RealBadAngel_)
- Updated buildbot scripts and added 64-bit buildbot (_sfan5_)
- Added support for directly starting a world by name from command line (_sapier_)
- Many performance improvements and memory usage reductions (_sapier_)
- Added support for Android 2.3 and later (_sapier_)

### Bug fixes

- Fixed objects being selected behind a node (_Novatux_)
- Fixed absence of images when compiled with `RUN_IN_PLACE=0` (_xyz_)
- Added option to link to OpenGL ES (_sfan5_)
- Fixed CMake list parsing in build (_hasufell_)
- Fixed cutting of multi-line error messages at half of second line in main menu dialog (_celeron55_)
- Created new instance of mesh every time it's required (_celeron55_)
- Escaped error messages in error dialog (_PilzAdam_)
- Added operating system to user agent (_proller_)
- Prevented auto-rotated nodes from replacing the nodes they were placed on (_ShadowNinja_)
- Added protection support to auto-rotated nodes (_ShadowNinja_)
- Prevented looking up node texts in a endless recursion loop (_sapier_)
- Set locale properly when built without gettext support (_celeron55_)
- Fixed Minetest's reliable UDP implementation (_sapier_)
- Compare values instead of pointers in Inventory::operator== (_kahrl_)
- Fixed some errors reported by clang static analyzer (_xyz_)
- Fixed win32 reading semaphore count not working (broke all queues) (_sapier_)
- Prevented player from jumping into nodes from below (_BlockMen_)
- Fixed cURL DLL not getting installed when sound was disabled (_sfan5_)
- Fixed error on mod download failure (_ShadowNinja_)
- Fixed use of previously deallocated EmergeManager (_kwolekr_)
- Fixed only half of unreliable queue being handled per step in worst case (_sapier_)
- Fixed player textures by adding '-' to list of allowed characters in media filenames (_sapier_)
- Fixed texture pack names corrupting mainmenu (_ShadowNinja_)
- Fixed crash when a error occurred in a globalstep callback (_ShadowNinja_)
- Fixed a heap-use-after-free in pause menu (_xyz_)
- Added checks for invalid user input for important settings (_kwolekr_)
- Fixed memory leak in database migration (_Selat_)
- Fixed invalid check for fread error on extracting zip (_sapier_)
- Fixed a unloaded but active block problem (_CiaranG_)
- Fixed rendering glitches when far from the center of the map (_Novatux_)
- Fixed race condition on exit to menu (_sapier_)
- Fixed generating winresource.o with build dir != source dir (_sfan5_)
- Fixed special characters in pause and message menu (_BlockMen_)
- Fixed game pause in singleplayer (_BlockMen_)
- Fixed "ghost stacks" created when a player clicks an item on the ground (_Novatux_)
- Fixed double sending of chat messages (_sapier_)
- Fixed bug in RemoteClient::GetNextBlocks (_celeron55_)
- Fixed crash when teleporting near unknown node (_BlockMen_)
- Fixed broken IPv4 serialization on win32 (_sapier_)
- Fixed invalid liquid lighting (_RealBadAngel_)
- Fixed wrong node texture rotation for facedirs 5 and 7 (_MetaDucky_)
- Fixed crash when trying to draw too many items from inventory in HUD (_celeron55, RealBadAngel_)
- Fixed a text border update bug (_ShadowNinja_)
- Added a hack to avoid a 2 second startup delay on local games (_sapier_)
- Fixed `player:set_animation()` in third person view (_BlockMen_)
- Fixed numeric underflow on calculating window size adjustment (_sapier_)
- Fixed heart + bubble bar size on different texture packs (_sapier_)
- Added a limit to node meta data resolving recursion (_ShadowNinja_)
- Fixed typo (std::encl) in src/gettext.cpp (_JakubVanek_)
- Fixed wielded index being greater than inventory size (_RealBadAngel_)
- Fixed chat overlaying full screen (_sapier_)
- Fixed build on big endian architectures (_mat8913_)
- Made lack of IPv6 non-fatal to unit tests (_sapier_)
- Fixed the Map and Rollback databases not closing (_sapier_)
- Fixed day/night passing at the wrong speed on some architectures (_sapier_)
- Handle missing tablecolumns[] (_kahrl_)
- Fixed wrong status text rectangle (_sapier_)
- Fixed GenericCAO not grabbing member objects, causing them to be deleted early (_sapier_)
- Fixed support for Max OSX (_mdoege_)
- Fixed regression in light calculation (_sapier_)
- Passed `pointed_thing` to `after_place_node` (_ShadowNinja_)
- Documented "wielditem" visual (_ShadowNinja_)
- Passed `pointed_thing` to `on_rightclick` (_Novatux_)
- Added force-loading (_Novatux_)
- Added `InvRef::get/set_lists()` (_ShadowNinja_)
- Mapgen V6: Added flag to stop mud flow (_kwolekr_)
- Allowed vertical axis particle rotation constraint (_khonkhortisan_)
- Used tables for adding particles, deprecated former way (_khonkhortisan_)
- Added formspec table (_kahrl_)
- Added `minetest.override_item` (_ShadowNinja_)
- Added reading of slice probability table from schematic descriptors (_kwolekr_)
- LuaVoxelManip: Added `get_param2_data` and `set_param2_data` (_kwolekr_)
- Added `pointed_thing` to `minetest.register_on_placenode` (_ShadowNinja_)
- Added `pointed_thing` to `minetest.register_on_punchnode` and `on_punch` callbacks (_ShadowNinja_)
- Added `player:set_sky()` with simple skybox support (_celeron55_)
- Added `player:override_day_night_ratio()` for arbitrarily controlling sunlight brightness (_celeron55_)
- Added `minetest.kick_player(name`, reason) (_sapier_)
- Added capability to read table flag fields from Lua API (_kwolekr_)
- Added `minetest.set_noiseparam_defaults()` (_kwolekr_)
- Added `force_placement` parameter to `minetest.place_structure` (_kwolekr_)
- Removed "Server -!- " prefix from player messages (_ShadowNinja_)
- Updated `set_mapgen_params` and `set_gen_notify` to use new flag format (_kwolekr_)
- Added `player:set_local_animations()` (_BlockMen_)
- Added `player:set_eye_offset()` (_MirceaKitsune, BlockMen_)
- Added support for function serialization to minetest.serialize (_ShadowNinja_)
- Added proper Lua API deprecation handling (_sapier_)
- Made dump2() return the serialized string, like dump() (_ShadowNinja_)
- Added item eat callback (_rubenwardy_)
- Added success and output return values to chat commands (_ShadowNinja_)
- Allowed custom liquids to have drops (_sfan5_)
- Made dropdown formspec elements send their values the same way that buttons do (_sapier_)
- Reworked tooltips, adding a tooltip element (_RealBadAngel_)
- Reworked the `glasslike_framed` drawtype (_RealBadAngel_)

### Vanilla game changes (minetest_game)

- Added desert cobblestone, dropped by desert stone (_brunob.santos, sfan5_)
- Added desert stone brick and sandstone brick stairs/slabs (_Amaz1_)
- Added extended doors mod (_PenguinDad, BlockMen_)
- Added glass panes and iron bars (_BlockMen_)
- Added TNT (_BlockMen_)
- Reworked farming mod, added API (_webdesigner97_)
- Upwards digging for papyrus and cactus (_casimir_)
- Additional mirrored recipes for axes (_marvok_)
- Bookshelves have an inventory (for books) (_arsdragonfly_)
- Added furnace protection and progress visualization (_Krock_)
- Added /sethome and /home commands (with “home” privilege) (_sfan5_)
- Added Mese and diamond hoes (_BlockMen_)
- Enabled jungles by default on new worlds (_sfan5, BlockMen_)
- Increased crafting output for stairs (4 → 6) (_Jonathon Station_)
- Made punching bones pick up items (_Krock_)
- Made items drop if no space for bones (_Krock_)
- Don't create bones when inventory is empty (_arsdragonfly_)
- Added cuboid wield hand (_paramat_)
- Many new textures, added inventory backgrounds (_BlockMen_)

### Master server (server list)

- Fixed null string escape in server list (_proller_)
- Made server announcing use multipart/form-data (_ShadowNinja_)
- Fixed boolean typing and alignment (_ShadowNinja_)
- Fixed code style, const-correctness, and types (_ShadowNinja_)
- Moved the master server to separate repository (_ShadowNinja_)
- Fixed seconds validity check (_ShadowNinja_)
- Made README more complete (_ShadowNinja, sfan5_)
- Made the hovered entry highlight in very light gray (_sfan5_)
- Switched to 'display' instead of 'visibility' to prevent the page from having white space at the bottom (_sfan5_)

## 0.4.8 → 0.4.9

0.4.9 was released on January 1, 2014.

### New features

**Logistic changes**

- Added SQLite rollback (_Mario Barrera & ShadowNinja_)
- Implemented HTTPFetch (_kahrl_)
- Replaced SimpleThread with JThread (_sapier_)
- Added handling for LuaErrors in Lua -> C++ calls on LuaJIT (_ShadowNinja_)
- Made SHA1::addBytes(..., 0) a no-op instead of an assertion failure (_kahrl_)

**Visual changes**

- Reworked shaders (_RealBadAngel_)
- Added configurable font shadow (_xyz_)
- Added Directional fog + horizon colors (_Taoki_)
- Removed FPS from window title (Doubles performance on some window managers) (_PilzAdam_)

**Other changes**

- Implemented modstore search tab and version picker (_sapier_)
- Added check for denied access in itemdef/nodedef/media fetch loop (_kahrl_)

### Bug fixes

- Fixed `line_of_sight()` (_sapier_)
- Fixed modstore/favourites hang by adding asynchronous Lua (_sapier_)
- Fixed LevelDB maps (_sfan5_)
- Fixed Lua mapgen override param handling (_kwolekr_)
- Fixed leak and possible segfault in `minetest.set_mapgen_params` (_kwolekr_)
- Fixed segfault in indev cave generation due to uninitialized variable (_kwolekr_)
- Added check for if width, height or start index of a list[] is negative (_PilzAdam_)
- Fixed single character formspec field labels (_BlockMen_)
- Added handling for Lua errors in `on_generate` callbacks (_kwolekr_)
- Update mapgen params in ServerMap after Mapgen init (_kwolekr_)
- Fixed wrong names for parallax settings in config example. (_RealBadAngel_)
- Fixed particle code ignoring return value of std::vector::erase(). (_kahrl_)
- Fixed `minetest.facedir_to_dir` when param2 is 5 or 7. (Again) (_Novatux_)
- Fixed InventoryList reading order (_ShadowNinja_)
- Initialize world before creating BanManager and RollbackManager (_ShadowNinja_)
- Fixed exception caused by destroying sockets on Server shutdown (_kwolekr_)
- Added area parameters back to `calc_lighting()` and `set_lighting()` (_kwolekr_)
- Added `get_light_data()` and `set_light_data()` to LuaVoxelManip (_kwolekr_)
- Added `minetest.swap_node` (_Novatux_)
- Assumed a selection box for fences (_0gb.us_)
- Decoration: Added schematic Y-slice probability support (_kwolekr_)
- Added sneak and `sneak_glitch` in `set_physics_override()` (_PilzAdam_)
- Used a table in `set_physics_override()` (_PilzAdam_)
- Added `on_prejoinplayer` callback (_kaeza_)
- Made `line_of_sight` return blocking node position (_stujones11_)
- Removed support for optdepends.txt (_ShadowNinja_)
- Added map feature generation notify Lua API (_kwolekr_)
- Added `minetest.write_json` (_ShadowNinja_)
- Log guilty node name when `allow_metadata_inventory_move/put/take` fails (_kahrl_)
- Fixed enum element name in Lua HUD code (position vs. pos) (_kaeza_)

## 0.4.7 → 0.4.8

0.4.8 was released on November 24, 2013.

### New features

**Big gameplay changes**

- Added drowning (_PilzAdam, RealBadAngel, BlockMen_)
- Added weather support (_proller_)

**Smaller gameplay tweaks**

- Added new sounds (_someone who can't decide if he wants to be called mitori or mito551_)
- Don't predict placing and removing nodes if interact privilege is missing (_PilzAdam_)

**Logistic changes**

- Clean up rendering code a bit (increases FPS by 5 to 10) (_Exio_)
- Added support for IPv6 (_matttpt_)
- Don't write player files all the time if they are not modified (_PilzAdam_)
- Made the main menu Lua based (_sapier, kahrl_)
- Change static ContentFeatures array into a vector (_rathgit, kahrl_)
- Allow multiple singleplayer games at the same time (_PilzAdam_)
- Added texture pack selection to main menu (_Novatux_)
- Don't write files directly but rather write to a temporary file that gets renamed after successfully written; fixes many causes of corrupted files (_PilzAdam_)
- Adjust the Lua API structure and improve header inclusion to decrease compile time (_kahrl_)
- Database abstraction, LevelDB support (_sfan5, wieszak, xyz_)
- Use the Settings Lua interface to read world.mt (_PilzAdam_)
- Use `engine.is_yes()` in mainmenu (_PilzAdam_)
- Always use builtin JThread library (_kwolekr_)
- Optimized `minetest.get_connected_players()` (_fairiestoy_)
- Removed `mapgen_air` alias (#935) (_0gb.us_)
- Raise the maximum node limit to 0x7fff (_ShadowNinja_)
- Shortened lines in falling node code (_ShadowNinja_)
- Moved the sapling growing and grass adding/removing ABMs to Lua (_Novatux_)
- Portability fixes for OpenBSD (and possibly NetBSD and others) (_Warr1024_)
- Accept hexadecimal and string values for seeds (_kwolekr_)
- Pass a errfunc to `lua_pcall` to get a traceback (_ShadowNinja_)
- Replaced print()s with minetest.log() in builtin (_PilzAdam_)
- Updated parameter index of `set_lighting()` (_kwolekr_)

**Visual changes**

- Added support for bumpmapping (_RealBadAngel_)
- Added diagonal liquid animation (_kahrl_)
- Damage updates and effects are now sent to other players (_PilzAdam_)
- Made fog depend on humidity (_proller_)
- Added git hash to version string in top left corner of window (_kahrl_)
- Added --version option (_kahrl_)
- Fixed `liquid_range`, fixing graphical glitches on old servers (_PilzAdam_)
- Added seed entry to world creation dialog (_kwolekr_)

**Other changes**

- Play `player_damage.ogg` when receiving damage and `player_falling_damage.ogg` on falling damage (_PilzAdam_)
- Added basic unicode support to the console in Linux (_Exio_)
- Added a setting for max loop count per step in liquid update (_PilzAdam_)
- Added math mapgen with fractal based worlds (_proller_)
- Disallow the name 'singleplayer' in a multiplayer server (_PilzAdam_)
- Added `max_objects_per_block` to minetest.conf to control the maximum number of static objects per block (_Novatux_)
- Removed broken farmesh (_kahrl_)
- Added `language` setting to `minetest.conf` which forces Minetest to use specified translation (_xyz_)
- Added configurable PRAGMA synchronous = (_proller_)
- Added curl, freetype and luaJIT to `CMAKE_BUILD_INFO` (_PilzAdam_)
- Lowered the default `max_users` from 100 to 15 (_ShadowNinja_)
- Removed doc/gpl-2.0.txt, add doc/lgpl-2.1.txt (_kahrl_)
- Master server update (_proller_)
- Moved new core devs to the "Core Developers" section of mainmenu (_Novatux_)
- Added ShadowNinja's email address to the main menu credits (_ShadowNinja_)
- Used a doT.js template for the serverlist (_ShadowNinja_)
- Added `default_privs` to master server and JS auto-load (_proller_)
- Added BlockMen to core dev list (_PilzAdam_)
- Added missing RequestQueue doc (_sapier_)
- Prevent enabling Shaders if Direct3D is used (_PilzAdam_)
- Fix my name (doesn't display correctly because of utf8 characters) (_Novatux_)

### Bug fixes

- Fixed `print(nil)` crashing the server (_kahrl_)
- Fixed output of profiler (F6) when using freetype (_kahrl_)
- Fixed bug where wrong item is selected when dropping something in the inventory on another stack (_kahrl_)
- Fixed lighting bug caused by disappearing lava (_PilzAdam_)
- Fixed /unban command crashing the server (_kahrl_)
- Fixed lighting bug with 6d facedir (_RealBadAngel_)
- Fixed and improved view range tuner (_celeron55_)
- Fixed and improved anticheat (_celeron55_)
- Fixed server getting completely choked up on even a little of DoS (_celeron55_)
- Fixed crack overlay for animated textures (_kahrl_)
- Added fallback font for Chinese, Japanese and Korean languages, the translations in those languages can now be displayed (_xyz_)
- Fixed most object duplication bugs (_celeron55_)
- Fixed hotbar padding at bottom (_BlockMen_)
- Fixed comments about length of server step (_ShadowNinja_)
- Fixed some warnings and other minor details (_kwolekr_)
- Re-fixed `hud_change` stat argument retrieval (_kwolekr & ShadowNinja_)
- Fixed wrong error message on invalid use of the formspec element `image_button` (_RealBadAngel_)
- Fixed object duplication bug (_celeron55_)
- Made unknown nodes stop falling nodes properly (_ShadowNinja_)
- Fixed ignoring of "diggable" property of nodes (_0gb.us_)
- Fixed invalid use of pointer to temporary object in JSON to Lua conversion (_sapier_)
- Fixed win32/msvc i18n (_sapier_)
- Fixed weather (_kwolekr_)
- Prevented shaders from being created when disabled (_kwolekr_)
- Fixed formspec background padding when clipped (_BlockMen_)
- Fixed array limit check when reading Lua specialtiles table (_MetaDucky_)
- Fixed invalid listname and listsize not handled correctly in `set_size` (_sapier_)
- Handle blank blocks in database (_kwolekr_)
- Fixed multi-caller support in RequestQueue (_sapier_)
- Fixed result of processed request being written to invalid (non-existent) ResultQueue if requesting thread timed out (_sapier_)
- Fixed gettext compile issues under win32 (_MetaDucky_)
- Made mapgen V6 respect `water_level` setting (_kwolekr_)
- Fixed usage of 'minetest' where 'engine' was intended (_ShadowNinja_)
- Fixed crash when pressing Enter key in formspec menu (_kahrl_)
- Fixed rename modpack button not working, fixes #1019 (_PilzAdam_)
- Don't continue trying to deserialize blank block data (_kwolekr_)
- Added in-game modstore to download mods from mmdb (_sapier_)
- Added `minetest.register_decoration()` (_kwolekr_)
- Added schematic support; new functions `minetest.place_schematic()` and `minetest.create_schematic()` (_kwolekr_)
- Separated formspecs of furnace and chests to provide override by mods (_BlockMen_)
- Added Lua VoxelManip (_kwolekr_) [http://forum.luanti.org/viewtopic.php?id=6396](http://forum.luanti.org/viewtopic.php?id=6396)
- Added vector helpers (_ShadowNinja_)
- Added `range` to item definition (_PilzAdam_)
- Added `after_use` to item definition (_Novatux_)
- Added `liquid_range` to node definition (_PilzAdam_)
- Added `collide_with_objects` to entity definition, to disable object <-> object collision (_PilzAdam_)
- Added `minetest.facedir_to_dir()` and 6d facedir support for `minetest.dir_to_facedir()` (_hdastwb_)
- Added gettext for `image_button` (_BlockMen_)
- Added `stepheight` to entity definition (_sapier_)
- Added support for multiple `wherein` nodes in `minetest.register_ore()` (_PilzAdam_)
- Added `minetest.register_on_cheat()` (_celeron55_)
- Added `automatic_face_movement_dir` to entity definition (_sapier_)
- Added `player:hud_set_hotbar_image()` and `player:hud_set_hotbar_selected_image()` (_PilzAdam, BlockMen_)
- Added percent scaling for HUD images (_BlockMen, kahrl_)
- Added `minetest.get_gametime()` (_Novatux_)
- Allowed manually specifying param2 in `minetest.item_place()` and return success (_PilzAdam_)
- Added `set_name()`, `set_count()`, `set_wear()` and `set_metadata()` to Lua ItemStack (_PilzAdam_)
- Added support for parameter `visual_scale` for drawtypes 'signlike' and 'torchlike' (_Sokomine_)
- Fixed `minetest.facedir_to_dir` when param2 is 5 or 7. (_Novatux_)
- Added `after_use` tool callback (_Novatux_)
- Added settings interface for Lua (_PilzAdam_)
- Moved tree growing and grass growing ABMs to Lua (_Novatux_)
- Added `minetest.register_on_craft()` and `minetest.register_craft_predict()` (_Novatux_)
- Added basic protection support to builtin (_ShadowNinja_)
- Added 6d facedir rotation prediction routine (_VanessaE_)
- Added wrapper for `minetest.rotate_and_place` (_Evergreen_)

**Formspec changes**

- `pwdfield`, `vertlabel`, `tabheader`, `dropdown` and `checkbox` (_sapier_)
- `<noclip>;<drawborder>;<pressed texture name>` options for `image_button` (_sapier, BlockMen_)
- `textlist` and `box` with color support (_sapier, sfan5_)
- `listcolors` and `bgcolor` (_BlockMen, kahrl_)
- `<auto_clip>` option for background images (_BlockMen_)
- Added option to scale image to percentage values (_BlockMen_)
- Send a `on_receive_fields` event when formspec is closed, with fields.quit = "true" (_Novatux_)
- Reworked formspecs (_BlockMen_)

## 0.4.6 → 0.4.7

0.4.7 was released on June 6, 2013.

### New features

**Big gameplay changes**

- Added snow, snow block, ice and dirt with snow (_PilzAdam_)
- Added sandstone bricks and desert stone bricks (_PilzAdam & VanessaE_)
- Added coal block, crafted out of 9 coal lumps (_Zeg9_)
- Added flowers to craft dyes; flowers and grass grow now on `dirt_with_grass` (_0gb.us, PilzAdam, VanessaE, ironzorg_)
- Added farming mod; wheat can be used to bake bread and cotton can be used to craft wool (_PilzAdam_) [http://forum.luanti.org/viewtopic.php?id=6067](http://forum.luanti.org/viewtopic.php?id=6067)

**Smaller gameplay tweaks**

- Added a little delay for falling nodes to update so that the objects don't spawn all at once (_PilzAdam_)
- Added private messaging with `/msg` (_ShadowNinja_)
- Added copper block (_RealBadAngel_)
- Swing the camera down when the player lands on the ground; disabled by default; `fall_bobbing_amount` in minetest.conf (_Taoki_)
- Node placement prediction now accounts for "wallmounted", "facedir" and `attached_node` nodes and only replaces `buildable_to` nodes (_kahrl, ShadowNinja & PilzAdam_)
- Added `disable_fire` setting to disable fire burning (_ShadowNinja_)
- Added damage to the hand in creative mode (_PilzAdam_)
- Added a little animation when changing the wielded item (_PilzAdam & blue42u_)
- Apples now fall when the tree decays (_PilzAdam & BlockMen_)

**Logistic changes**

- Added mapgen v7; not usable currently (_kwolekr_)
- Added support for LuaJIT, makes mod execution much faster (_RealBadAngel_)
- Move cave generation to cavegen.cpp and restructure it into a class (_kwolekr_)
- Added icons to select games in menu; `menu/menu_<background/overlay/header/footer>.png` of selected game is used in the main menu (TP can use `<gameid>_menu_<background/overlay/header/footer>.png`) (_celeron55_)
- Added `--videomodes` option to show available video modes (_kahrl_)
- Added ability to play `main_menu.ogg` (`main_menu.<1-9>.ogg` are supported too; they are chosen randomly if present) in main menu (_RealBadAngel_)
- Drop common mods system, _Survival_ and _Build_ game; minetest*game includes all common mods and the bones mod from \_Survival* now (_PilzAdam_) [http://forum.luanti.org/viewtopic.php?id=6034](http://forum.luanti.org/viewtopic.php?id=6034)
- Changed mod system a bit: All user mods are installed in `$path_user/mods/` now; they can be enabled per world in the configure world window or in `world.mt` with `load_mod_<modname>` (_PilzAdam_) [http://forum.luanti.org/viewtopic.php?id=6066](http://forum.luanti.org/viewtopic.php?id=6066)
- Split `init.lua` of the default mod into several files (_PilzAdam_)
- Moved scriptapi to a subfolder (_sapier, celeron55 & kahrl_)

**Visual changes**

- Changed "unknown block" texture to "unknown node" (_khonkhortisan_)
- Changed textures of sand, desert sand and desert stone (_VanessaE_)
- `crosshair.png` is used instead of the normal crosshair if present (_dannydark & Exio4_)
- Added progress bar and clouds to loading screen (_Zeg9_)
- Added new textures for all metal and diamond blocks (_Zeg9_)
- Added new Minetest header (_BlockMen_)

**Other changes**

- Added `mouse_sensitivity` option (_Exio4_)

### Bug fixes

- Check if the address field is empty when hitting enter on the multiplayer tab (_ShadowNinja_)
- Limit speed in collisionMoveResult for avoiding hangs (_Exio4_)
- Fixed camera "jumping" when attached and the parent goes too fast (_Zeg9_)
- Fixed nick completion in chat console with the tab key (_PilzAdam_)
- Do not always move fast in water and ladders when `aux1_descend` it true (_Taoki_)
- Fixed **a lot** memory leaks (_sapier, PilzAdam, kahrl, kwolekr_)
- Fixed import of older maps (_kwolekr_)
- Fixed black trees (_kwolekr_)
- Fixed small objects colliding with themselves (_sapier_)
- Fixed `get_craft_recipe()` and `get_all_craft_recipes()` (_RealBadAngel_)
- Fixed spawning too high above ground (_kwolekr_)
- Fixed object -> player collision (_sapier_)
- Fixed favorite server list in globally installed versions of Minetest `(RUN_IN_PLACE=0)` (_Zeg9_)
- Fixed favorite server list on windows (_sfan5_)
- Fixed handling of mods in games in the configure world GUI (_kahrl_)
- Fixed static data of objects not being stored correctly on deactivation (_sapier_)
- Removed _Meshbuffer ran out of indices_ limitation (_kahrl_)
- Fixed `isBlockInSight()` for higher FOV (_Warr1024_)
- Don't teleport back when a player is detached or turns free move off and holds shift (_PilzAdam_)
- Fixed bug where you need to move the mouse after closing a menu (_kahrl_)
- Reduced `/clearobjects` memory consumption; `max_clearobjects_extra_loaded_blocks` in minetest.conf (_kahrl_)
- Corrected segfault when registering new biomes (_sweetbomber_)
- Reduced video memory consumption by not generating unnecessary `[forcesingle` textures (_kahrl_)
- Close console when it loses focus but it is still on screen (_Exio4_)
- Added `player:set_physics_override()` to set per-player physics (_Taoki & PilzAdam_)
- Use `node_box` for `selection_box` if `drawtype = "nodebox` and `selection_box = nil` (_kaeza_)
- Added `minetest.env:line_of_sight()` and `minetest.env:find_path()` (_sapier_)
- Added API functions to add elements to the HUD of players (_blue42u, kwolekr & kaeza_)
- Added option to not prepend "Server -!- " to messages sent with `minetest.chat_send_player()` (_ShadowNinja_)
- Added `minetest.get_player_ip()` (_ShadowNinja_)
- Added `use_texture_alpha` in node definition to use alpha channel of node texture (_kwolekr_)
- Added `glasslike_framed` node drawtype (_RealBadAngel_)
- Added optional dependencies and different mod name conflict handling (_kahrl_)
- Use group `soil` for nodes where saplings can grow on (_ShadowNinja_)
- Nodes with drawtype `raillike` connect to all other nodes with the same drawtype if they are in the `connect_to_raillike` group (_Jeija_)
- Env functions are now in the global minetest table; that means they are called via `minetest.<function>` instead of `minetest.env:<function>` (_sapier, celeron55 & kahrl_)
- Added `obj:set_hotbar_itemcount()` (_kahrl_)

## 0.4.5 → 0.4.6

0.4.6 was released on April 3, 2013.

### New features

**Big gameplay changes**

- Added lava cooling near water; lava source turns into obsidian, flowing lava turns into stone (_PilzAdam_)
- Added jungle wood (with stairs and slabs), jungle leaves and jungle saplings (_PilzAdam_)
- Added obsidian, obsidian shards and obsidian glass (_PilzAdam & jojoa1997_)
- Added grass (5 different heights) (_PilzAdam_)
- Added growing for papyrus (on dirt and grass near water) and cactus (on sand) (_PilzAdam_)
- Added stone bricks crafted from 4 stones (_PilzAdam_)
- Added gold (_PilzAdam_)
- Added diamonds and diamond tools, which are slightly faster and wear out slower than mese tools (_PilzAdam_)
- Added mese axe, shovel and sword; mese pick is not the ultimate tool anymore (_PilzAdam_)
- Added copper, bronze and bronze tools; bronze can be crafted with copper ingot and steel ingot; bronze tools have same digging times but more uses than steel tools (_PilzAdam_)

**Smaller gameplay tweaks**

- 3 nodes now give 6 slabs instead of 3 (_PilzAdam_)
- Wooden stairs and slabs are now flammable (_PilzAdam_)
- Lava is not renewable anymore (_PilzAdam_)
- It is not possible anymore to place non-fuel items in the fuel slot or any item in the output slots of the furnace (_PilzAdam_)
- Falling nodes now destroy solid `buildable_to` nodes (_Splizard_)
- Added ability for buckets to pick up flowing water when `liquid_finite` is enabled (_ShadowNinja_)
- Use right click to place liquids with buckets; added description for buckets (_PilzAdam & ShadowNinja_)
- Fixed furnace infotext saying "Furnace out of fuel" when placing a fuel but no item to cook into it (_PilzAdam_)
- Made Mese ores a bit more rare; made Mese blocks very rare (_PilzAdam_)
- Added object <-> object collision (_sapier_)

**Map generation changes**

- Re-added dungeons (disabled by default, enable with "dungeons" flag in `mg_flags` setting) (_kwolekr_)
- Speed up lighting a lot (_kwolekr_)
- Re-added jungles (disabled by default, enable with "jungles" flag in `mg_flags` setting) (_kwolekr_)
- Generate apple trees (_kwolekr_)
- Moved ore generation back to core; improved ore generation speed (_kwolekr_)
- Added `singlenode` mapgen (_celeron55_)
- Added a new map generator called `indev` (float islands at 500+, rare HUGE caves, near edges: higher mountains, larger biomes) (_proller_)

**Visual changes**

- Changed textures of cobblestone and mossy cobblestone (_PilzAdam_)

**Logistic changes**

- Split scriptapi.cpp into more files (_sapier_)
- Migrate to STL containers/algorithms (_xyz_)
- Added the pseudo game _common_ with _bucket_, _default_, _stairs_, _doors_ and _fire_ mods included; deleted those mods from `minetest_game` (_celeron55 & PilzAdam_)
- Added a checkbox for finite liquids to settings menu (_proller_)

**Other changes**

- Use moving clouds as background for the main menu (_Krisi & ShadowNinja_)
- `minetest.env:find_nodes_near()` optimized to be 11.65x faster, ServerEnvironment step CPU consumption cut in half (_kwolekr_)

### Bug fixes

- Fixed build with ogles2 driver (_proller_)
- Fixed `new_style_water` (shaders are not used for this anymore) (_PilzAdam_)
- Fixed `backface_culling` in tiledef; both sides of flowing liquids are now visible (_doserj_)
- Hopefully fix node replacement bug (where the node that is pointed at is replaced) (_0gb.us_)
- Added `minetest.get_all_craft_recipes(output)` (_RealBadAngel_)
- Allow any character in formspec strings with escape characters (_kwolekr_)
- Added ability to pass multiple parameters to `minetest.after()` (_Jeija_)
- Added `player:set_look_yaw()` and `player:set_look_pitch()` (_RealBadAngel_)
- Added ability to load mods from the pseudo game _common_ via `common_mods` in game.conf (_celeron55_)
- Added support for a minetest.conf file in games, which override the default values (_celeron55_)
- Added 6d facedir to rotate nodes with _facedir_ drawtype (_RealBadAngel_)
- Added function and wrapper to predict and assign 6d rotation via `minetest.rotate_and_place()` (_VanessaE and EvergreenTree_)
- Added `minetest.add_particle()`, `minetest.add_particlespawner()` and `minetest.delete_particlespawner()` (_Jeija_)
- Added `minetest.register_ore()` to let the engine generate the ores; `default.generate_ore()` is now deprecated (_kwolekr_)
- New damage system (_PilzAdam & celeron55_)
- Added `place` field to sound table of tools (_PilzAdam_)

## 0.4.4 → 0.4.5

0.4.5 was released on March 4, 2013.

### New features

**Big gameplay changes**

- Added Mese crystals and Mese crystal fragments (crafted from 1 Mese crystal); Mese blocks can be crafted with 9 Mese crystals; Mese pickaxes are now crafted using Mese crystals; old Mese equals the new Mese block and is still generated at altitudes -1024 and below (_VanessaE & PilzAdam_)
- Doors must now be right clicked to be opened (_PilzAdam_)
- Flying through walls now requires the "noclip" privilege and noclip mode must be enabled by pressing H (_PilzAdam_)
- Added a list of servers to the "Multiplayer" tab of the main menu (_Jeija_)
- Added a mod selection menu (_doserj_)
- Jungle grass now spawns naturally again (_PilzAdam_)
- Added finite liquid support, experimental and disabled by default (_proller_)

**Smaller gameplay tweaks**

- Locked chest contents are now only shown to their owner (_PilzAdam_)
- Added ability to write several lines on a sign (_PilzAdam_)
- When sneaking, the current node/item will always be used when right clicking even if pointing a chest or a furnace (_Jeija_)
- In creative mode, hand now breaks everything nearly instantly and nodes/items are infinite (_PilzAdam_)
- Player physics are now tweakable by server admin (_Taoki_)
- Fast mode can now be used in liquids and in climbable nodes (_kwolekr_)
- Fire is now "buildable to" (_Casimir_)
- To fly at "fast" speed, the "use" key must now be held if using shift to descend (_PilzAdam_)
- Added upside down stairs and slabs (_PilzAdam_)
- Added ability to switch to `fly_mode` when double-tapping space bar, disabled by default; can be enabled in the key change menu (_PilzAdam_)
- Tweaked damage and punch times of weapons, tools and hand (_Calinou_)
- Added repeated right clicking when holding the right mouse button, see `repeat_rightclick_time` setting in minetest.conf (_Jeija_)

**Map generation changes**

- Added L-system tree generation (_RealBadAngel & dannydark_)
- Map generation is now slightly faster and can be tweaked in minetest.conf (_kwolekr_)
- Added optional flat map generation, with and without trees (_kwolekr_)

**Visual changes**

- Mese pickaxe now has a new texture, which is more yellow (_Jordach_)
- Tweaked dirt texture so that it tiles better; improved lump and ingot textures; added fake shading to the default player texture (_Iqualfragile & GloopMaster & Jordach_)
- Added particles when digging blocks (_Jeija & PilzAdam_)
- The selection box of stairs now fits the stairs (_PilzAdam_)
- If damage is disabled, damage screen is disabled and health is not shown on the HUD (_PilzAdam_)
- Damage screen is now red fade instead of constant red; camera now tilts when receiving damage (_Jeija & PilzAdam_)
- Added `selectionbox_color`, `crosshair_color` and `crosshair_alpha` minetest.conf settings for changing selection outline color, crosshair color and crosshair opacity respectively (_Exio4_)

**Logistic changes**

- Minetest-c55 is now named Minetest
- Less stuff is now put in debug.txt by default, change with debug_log_level, default is 2
- Texture atlas is now disabled by default (_kwolekr_)
- Added and updated language translations; French, German, Portuguese, Polish and Spanish translations are 100% complete (_Calinou, kaeza, PilzAdam, sfan5, xyz, kotolegokot, pandaro, Mito551, Shen Zheyu, sub reptice, elagin, KikaRz and socramazibi_)
- Added support for downloading media from a server using cURL which is faster, disabled by default (_Ilya Zhuravlev_)

### Bug fixes

- Walking on stairs, slabs and glass now makes sounds (_PilzAdam & dannydark_)
- Fixed and cleaned EmergeThread around a bit (_kwolekr_)
- Punching entities and players with shovels and pickaxes now deals damage (_Calinou_)
- Fixed some caves having too many dead ends (_unknown_)
- Fixed the looks of some plantlike nodes by using two long planes instead of four shorter planes (_doserj_)
- Grass no longer turns into dirt below unloaded blocks (_PilzAdam_)
- Fixed a crash when clicking "Configure" when no world is selected in Singleplayer menu (_doserj_)
- Fixed dropped item collision with nodeboxes (_jordan4ibanez_)
- Fixed a glitch where the player gets liquids in his inventory when a server lags (_PilzAdam_)
- Added ability to change the itemstack in placenode callbacks (_PilzAdam_)
- Added ability to create multi-line text fields in formspecs (_Jeija_)
- Add `on_rightclick(pos, node, clicker)` callback for nodes (_PilzAdam_)
- Added `minetest.show_formspec(playername, formspec)` to show formspecs via Lua (_sapier_)

## 0.4.3 → 0.4.4

0.4.4 was released on December 6, 2012. 0.4.4-d1 (an interim release made due to a protocol change) was released on January 2, 2013.

### New features

- Added animated 3D player and a new default skin, the default model also supports Minecraft skins (_Taoki, skin by Jordach_)
- Added shaders support (can be disabled in Settings menu), makes water a bit smaller than a full block, makes lighting look prettier (_kahrl and celeron55_)
- New default doors mod: doors have a 3D look, ability to create "double doors" added, added locked steel doors (only the owner of the door can open/close it) (_PilzAdam_)
- Improve map generation speed a lot (_hmmmm_)
- Day-night transitions are now smoother (_celeron55_)
- Water textures are now animated (_RealBadAngel (textures) and PilzAdam_)
- Added on-demand item previews (reduces load time/RAM usage), disabled by default (_celeron55_)
- Added 3D anaglyph support (red-cyan glasses) (_xyz_)
- Fire is now animated and causes damage to players (_PilzAdam, Muadtralk (textures)_)
- Tweaked some textures: apple, nyan cat, bricks, papyrus, steel sword (_Calinou, VanessaE_)
- Tweaked digging animation (no more mining with the tip of your pickaxe!) (_jordan4ibanez_)
- Changed apple, sapling and papyrus selection box size to be smaller (_VanessaE_)
- Players who do not move no longer send their positions to save bandwidth (_Taoki_)
- Make steel block and brick drop themselves when dug and make them craftable back into their materials (_PilzAdam_)
- Glass now makes a sound when broken (_PilzAdam_)
- Dead players are now visible (_Taoki_)
- Changed default server tick to 0.1 second, decreasing server CPU usage (_celeron55_)
- Clients now send their position every 0.1 second too, making other player movement look smoother (_celeron55_)
- Use of /grant and /revoke commands is now logged (_dannydark_)
- Added ability for server to tweak amount of bandwidth used to upload mods to clients (_celeron55_)

### Bug fixes

{{% comment %}} cspell:disable {{% /comment %}}

- Fixed falling sand and gravel sometimes incorrectly landing (_PilzAdam_)
- Fixed empty bucket being named "emtpy bucket" (khonkhortisan and PilzAdam)
- Fixed slab to full block transformation (_PilzAdam_)
- Fixed smooth lighting between MapBlocks (_celeron55_)
- Prevent some blocks (leaves, falling sand and gravel) from giving air when dug when they disappear as you mine them (_PilzAdam_)
- Fixed papyruses and cacti growing inside trees (_PilzAdam_)
- Fixed flowing liquid animation direction calculation (_celeron55_)
- Fixed wielditem entity drawtype brightness control (_celeron55_)
- Fixed ObjectRef:punch() (_celeron55_)
- Fixed a rare bug in leaf decay (_PilzAdam_)
- Fixed trees growing into any type of node (_xyz_)
- Fixed server crashing when "/clearpassword" is typed without an argument (_Uberi_)
- Head no longer shifts downwards when you are inside transparent blocks such as glass or nodeboxes (_Calinou_)
- Directories beginning with a "." are now ignored when searching for mods on Windows (_matttpt_)
- Fixed the automagic render distance tuner (_celeron55_)
- Added 3D model support for entities (_Taoki_)
- Added attachment support (so that entities can "ride" other entities) (_Taoki_)
- Backgrounds and images can now be used in formspecs (_RealBadAngel_)
- Liquids can now be made non-renewable (_xyz_)
- Added `nodedef.on_blast()` to `lua_api.txt` in order to support chained explosions of any explosives (_celeron55_)
- Allow transparent image buttons (_khonkhortisan_)
- Added shutdown hook interface to Lua API (_matttpt_)
- Added `attached_node` group to make nodes which are not attached to any other walkable node drop (_PilzAdam_)

{{% comment %}} cspell:enable {{% /comment %}}
