---
title: ModStorage
aliases:
  - /api/classes/modstorage
---

# ModStorage

ModStorage is a per-world, per-mod persistent string key-value store implementing all methods of [MetaData](/for-creators/api/classes/metadata/).

The granularity of the persisted snapshots is determined by the `map_save_interval` setting.

## Backends

Two backends are available for ModStorage: JSON and SQLite3.

{{< notice warning >}}
The JSON backend is incapable of saving raw binary data due to JSON restrictions. Even though the SQLite3 backend supports arbitrary bytestrings, you may not rely on saving arbitrary bytestrings to work, since you can't ensure that the SQLite3 backend is being used.
{{< /notice >}}

If the SQLite3 backend is used, it is usually more efficient to leverage the key-value store than to store fully serialized data structures; fully serializing the data takes linear time in the size of the data whereas updating the key-value store only takes linear time in the size of the changes with the SQLite3 backend; for the JSON backend it is irrelevant - it has to fully serialize the data every map save interval anyways, increasing the risk of data loss if writing the file fails due to a hard crash (or freeze) of the Luanti server process.

## `core.get_mod_storage()`

Must be called at load time.

**Returns:**

- `storage` - ModStorage: Private ModStorage object for the currently loading mod

## Example

A basic greeting mod with a persistent greeting might look as follows:

```lua
local storage = core.get_mod_storage()

-- Send the greeting to joining players
core.register_on_joinplayer(function(player)
	local greeting = storage:get("greeting")
	if greeting then
		core.chat_send_player(player:get_player_name(), greeting)
	end
end)

-- Allow moderators to change the greeting
core.register_chatcommand("/set_greeting", {
	params = "<greeting>",
	description = "Sets the greeting",
	privs = {server = true},
	func = function(name, param)
		param = param:trim() -- MT-provided string.trim
		storage:set_string("greeting", param)
		return true, param == "" and "Greeting cleared." or "Greeting set."
	end
})
```
