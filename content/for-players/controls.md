---
title: Controls
aliases:
  - /Controls
  - /controls
---

# Controls

This is an overview of all controls used in Luanti.

## Changing controls

There are two ways to change the controls: Either by using the options menu accessible inside the game or by editing [minetest.conf](/for-players/minetest-conf). See minetest.conf.example to learn the setting names. Note that some controls are fixed and cannot be changed at all.

## Game controls

### PC

{{< notice note >}}
Starting from 5.12.0, keybindings are defined based on the scancode (i.e. location on the keyboard) instead of the keycode ("marking on the keycap") of the keys. The default controls shown below are based on the US layout and may be different for other layouts. For example, the default key for zooming is "Y" on the German layout, and the default key for moving forward is "Z" on the French layout.
{{< /notice >}}

The PC version of Luanti uses mouse and keyboard. These are the controls of the PC version:

| Action                                               | Default control               | Changeable in-game | minetest.conf setting             | Comment                                                                                                                                                                                                                                                                                          |
| ---------------------------------------------------- | ----------------------------- | ------------------ | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Movement                                             |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Look around                                          | Move mouse                    | No                 | (none)                            |                                                                                                                                                                                                                                                                                                  |
| Move forward                                         | W                             | Yes                | keymap_forward                    |                                                                                                                                                                                                                                                                                                  |
| Move to the left                                     | A                             | Yes                | keymap_left                       |                                                                                                                                                                                                                                                                                                  |
| Move to the right                                    | D                             | Yes                | keymap_right                      |                                                                                                                                                                                                                                                                                                  |
| Move backwards                                       | S                             | Yes                | keymap_backward                   |                                                                                                                                                                                                                                                                                                  |
| Toggle pitch move mode                               | (none)                        | Yes                | keymap_pitchmove                  | See #Movement modes.                                                                                                                                                                                                                                                                             |
| Toggle fast mode                                     | J                             | Yes                | keymap_fastmove                   | See #Movement modes. Requires the "fast" privilege.                                                                                                                                                                                                                                              |
| Toggle fly mode                                      | K                             | Yes                | keymap_freemove                   | See #Movement modes. Requires the "fly" privilege.                                                                                                                                                                                                                                               |
| Toggle noclip mode                                   | H                             | Yes                | keymap_noclip                     | See #Movement modes. Requires the "noclip" privilege.                                                                                                                                                                                                                                            |
| Aux1                                                 | E                             | Yes                | keymap_aux1                       | Makes you run faster when in fast mode. If the setting aux1_descends is true, this control also makes you descend in liquids and ladders. Some mods utilize this key to enable special actions (e.g. sprinting).                                                                                 |
| Jump / Move up                                       | Space                         | Yes                | keymap_jump                       | You will move upwards instead of jumping if you are climbing, swimming or using fly mode.                                                                                                                                                                                                        |
| Sneak / Move down                                    | Left Shift                    | Yes                | keymap_sneak                      | You will move downwards instead of sneaking if you are climbing, swimming or using fly mode.                                                                                                                                                                                                     |
| Toggle automatic forwards                            | (none)                        | Yes                | keymap_autoforward                | While this mode is enabled, this acts as if the forwards key is pressed all the time.                                                                                                                                                                                                            |
| World interaction                                    |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Punch / mine                                         | Left mouse button             | No                 | keymap_dig                        |                                                                                                                                                                                                                                                                                                  |
| Use / build                                          | Right mouse button            | No                 | keymap_place                      | If the pointed thing is usable (example: Chest), you use it, otherwise you attempt to build at this block                                                                                                                                                                                        |
| Build                                                | Left Shift+Right mouse button | No                 | keymap_sneak, keymap_place        | Use this to build at usable blocks                                                                                                                                                                                                                                                               |
| Select next/previous item stack in hotbar            | Roll mouse wheel              | No                 | (none)                            |                                                                                                                                                                                                                                                                                                  |
| Select previous item stack in hotbar                 | B                             | Yes                | keymap_hotbar_previous            |                                                                                                                                                                                                                                                                                                  |
| Select next item stack in hotbar                     | N                             | Yes                | keymap_hotbar_next                |                                                                                                                                                                                                                                                                                                  |
| Select item stack in hotbar directly                 | 0-9                           | No                 | keymap_slot1 - keymap_slot32      |                                                                                                                                                                                                                                                                                                  |
| Drop wielded item stack                              | Q                             | Yes                | keymap_drop                       |                                                                                                                                                                                                                                                                                                  |
| Drop 1 item of wielded item stack                    | Left Shift+Q                  | Yes                | keymap_sneak, keymap_drop         |                                                                                                                                                                                                                                                                                                  |
| Camera                                               |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Select camera                                        | C                             | Yes                | keymap_camera_mode                | Available cameras are (in this order): first person view, third person view from the back, third person view from the front                                                                                                                                                                      |
| Toggle cinematic mode                                | (none)                        | Yes                | keymap_cinematic                  | In cinematic mode, the camera will not immediately follow your movements, instead it will quickly “catch on”, so the movement of the camera looks a bit like the movement of an actual camera                                                                                                    |
| Zoom in at the crosshair                             | Z                             | Yes                | keymap_zoom                       | Usage of zoom can be restricted by game or mod. By default, zooming is only allowed in Creative Mode                                                                                                                                                                                             |
| Graphics                                             |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Enable/disable fog                                   | F3                            | Yes                | keymap_toggle_force_fog_off       |                                                                                                                                                                                                                                                                                                  |
| Increase minimal viewing distance                    | +                             | Yes                | keymap_increase_viewing_range_min |                                                                                                                                                                                                                                                                                                  |
| Decrease minimal viewing distance                    | -                             | Yes                | keymap_decrease_viewing_range_min |                                                                                                                                                                                                                                                                                                  |
| Toggle far view                                      | (none)                        | Yes                | keymap_rangeselect                | Far view allows to view things without a distance limitation. Warning: This can severely impact Luanti's performance; use this only briefly or for testing                                                                                                                                       |
| Take a screenshot                                    | F12                           | Yes                | keymap_screenshot                 |                                                                                                                                                                                                                                                                                                  |
| Heads-up Display                                     |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Show/hide inventory menu                             | I                             | Yes                | keymap_inventory                  |                                                                                                                                                                                                                                                                                                  |
| Show/hide HUD                                        | F1                            | Yes                | keymap_toggle_hud                 |                                                                                                                                                                                                                                                                                                  |
| Show/hide chat log                                   | F2                            | Yes                | keymap_toggle_chat                |                                                                                                                                                                                                                                                                                                  |
| Toggle minimap                                       | V                             | Yes                | keymap_minimap                    | Usage of minimap can be restricted by game or mod                                                                                                                                                                                                                                                |
| Toggle minimap shape (square or circle)              | Left Shift+V                  | Yes                | keymap_sneak, keymap_minimap      |                                                                                                                                                                                                                                                                                                  |
| Open/close console                                   | F10                           | Yes                | keymap_console                    |                                                                                                                                                                                                                                                                                                  |
| Abort / close window / open pause menu / quit Luanti | Esc                           | No                 | (none)                            |                                                                                                                                                                                                                                                                                                  |
| Sound                                                |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Decrease volume                                      | (none)                        | Yes                | keymap_decrease_volume            |                                                                                                                                                                                                                                                                                                  |
| Increase volume                                      | (none)                        | Yes                | keymap_increase_volume            |                                                                                                                                                                                                                                                                                                  |
| Toggle mute                                          | M                             | Yes                | keymap_mute                       |                                                                                                                                                                                                                                                                                                  |
| Commands and chat                                    |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Open chat window                                     | T                             | Yes                | keymap_chat                       | You need the “shout” privilege to chat                                                                                                                                                                                                                                                           |
| Start issuing a server command                       | /                             | Yes                | keymap_cmd                        |                                                                                                                                                                                                                                                                                                  |
| Start issuing a local command                        | .                             | Yes                | keymap_cmd_local                  | Local commands are part of client mods                                                                                                                                                                                                                                                           |
| Debugging (for developers)                           |                               |                    |                                   |                                                                                                                                                                                                                                                                                                  |
| Enable/disable camera update                         | F4 or none                    | No                 | keymap_toggle_update_camera       | Only useful for developers. If disabled, the landscape drawn around you will not be updated as you look around. This action only has a default key binding in the developer version of Luanti; in official releases there's no default key binding because this can be very confusing to players |
| Enable/disable debug display                         | F5                            | No                 | keymap_toggle_debug               | Also shows your coordinates                                                                                                                                                                                                                                                                      |
| Enable/disable mapblock bounds view                  | &lt;none&gt;                  | Yes                | keymap_toggle_block_bounds        | This displays the outlines of mapblocks, 16×16×16 portions of the map                                                                                                                                                                                                                            |
| Enable/disable profiler                              | F6                            | No                 | keymap_toggle_profiler            | Only useful for developers                                                                                                                                                                                                                                                                       |
| Write stack traces into debug.txt                    | (none)                        | No                 | keymap_print_debug_stacks         | Only useful for developers                                                                                                                                                                                                                                                                       |

### macOS

Same as for PC, with one difference: If you have a 1-button mouse, you can emulate a **right click** with a two finger tap on the trackpad.

### Mobile devices (Android / iOS)

The controls on mobile devices are severely restricted compared to the PC and you only have very basic controls. You can't do everything a PC player could do.

The [touchscreen](#touchscreen) is used for everything.

### Touchscreen

{{< notice note >}}
Touchscreen support was mobile (Android) only till version 5.5.0 when [desktop support](https://github.com/luanti-org/luanti/pull/10729) was added. Runtime
toggling of touchscreen controls was [added](https://github.com/luanti-org/luanti/pull/14075) in 5.9.0. Rudimentary detection was
[added](https://github.com/luanti-org/luanti/pull/14400) in 5.9.0, with [full support](https://github.com/luanti-org/luanti/pull/14542) being added
in 5.10.0.
{{< /notice >}}

In release 5.10.0 the touchscreen interface was improved to use a grid layout for controls with text being listed under the controls so users no longer
had to guess what they did.

{{< video src="https://samsinventory.docs.luanti.org/files/videos/touchscreen-grid.mp4" type="video/mp4" >}}

In release 5.11.0 an editor was added for rearranging the controls.

{{< video src="https://samsinventory.docs.luanti.org/files/videos/touchscreen-editor.mp4" type="video/mp4" >}}

{{< notice warning >}}
The controls listed below maybe outdated.
{{< /notice >}}

| Action                  | Control                                      |
| ----------------------- | -------------------------------------------- |
| Look around             | Touch screen and slide finger                |
| Use / build             | Double-tap                                   |
| Punch / mine            | Long tap                                     |
| Chat                    | Press on-screen button in left upper corner  |
| Jump                    | Press on-screen button in right lower corner |
| Sneak                   | Press on-screen button in right lower corner |
| Move left/up/right/down | Press on-screen button in left lower corner  |
| Display inventory       | Press on-screen button in left lower corner  |

Touchscreen controls when a formspec(such as a menu or inventory) is displayed:

- double tap outside menu area: close menu
- tap on an item stack: select that stack
- tap on an empty slot: if you selected a stack already, that stack is placed here
- drag and drop: touch stack and hold finger down, move the stack to another slot, tap another finger while keeping first finger on screen --> places a single item from dragged stack into current (first touched) slot

### Controllers and Gamepads

You'll need to use an external program to bind a controller. See [gamepads](/for-players/gamepads) for more info.

## Movement modes

Along with the normal controls, there are three so-called “movement modes” to change the way the player moves.

### Pitch move mode

If this mode is activated, the movement keys will move you relative to your current view pitch (vertical look angle) when you're in a liquid or in fly mode.

Because this movement mode may be confusing for newcomers, it doesn’t have a key assigned by default, so if you want to use it, you first have to set one manually in your controls settings.

### Fast mode

If this mode is activated (**default key: J**), it allows the player to move faster. If the fly mode is not activated, the player can run faster using the Aux1 key, which is E normally. If the fly mode is activated, the player will _fly_ faster instead.

_The “fast” privilege is required to use this._

### Fly mode

If this mode is activated (**default key: K**), the effects of gravity do not apply to the player anymore. This slightly changes the controls: The _jump_ key (**default: Space**) will cause the player to rise and the _sneak_ key (**default: Shift**) to sink.

_The “fly” privilege is required to use this._

### Noclip mode

If this mode is activated (**default key: H**) along with fly mode, the player can fly through walls. If fly mode is not activated as well, noclip mode has nearly no effect; it only prevents screen blackening when the player's head is inside a solid block.

_The “noclip” privilege is required to use this._

## Changing controls in `minetest.conf`

With `minetest.conf`, you can change the controls which are unavailable in the settings menu. See [minetest.conf#Controls](/for-players/minetest-conf/#controls) for more information.

## Inventory controls

See [Inventory#Controls](/for-players/inventory/#controls).

## Console controls

See [Console#Controls overview](/for-players/console/#controls-overview).
