---
title: Keeping world compatibility
aliases:
  - /keeping-world-compatibility
---

If your game or mod is stable and played by many players then it is a good idea to keep compatibility with old worlds as much as possible, in order to prevent unknown or glitched nodes showing up in players worlds.

## Aliases

Nodes and items support itemstring aliases, allowing you to alias an outdated itemstring to a new one.

{{% comment %}} cspell:disable {{% /comment %}}

```lua
core.register_alias('mymod:old_item_name', 'mymod:new_item_name')
```

{{% comment %}} cspell:enable {{% /comment %}}

This is also very useful for keeping compatibility when refactoring and/or namespacing technical mod names in a game:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
for _, item in ipairs {
	"stone", "cobble", "dirt", "grass", -- etc...
} do
	core.register_alias('oldmodname:'..item, 'game_newmodname:'..item)
end
```

{{% comment %}} cspell:enable {{% /comment %}}

## Loading Block Modifiers

Loading Block Modifiers (LBMs) can be used to run a callback function on any given nodes in mapblocks that get loaded and activated. They can be useful for doing more complex migrations of nodes that don't just require an itemstring alias, e.g. updating a node's formspec:

{{% comment %}} cspell:disable {{% /comment %}}

```lua
core.register_lbm({
	label = "Update Cool Block formspec to v2",
	name = "mymod:update_formspec_coolblock_v2",
	nodenames = {"mymod:coolblock"},
	run_at_every_load = false,
	action = function(pos, node)
		local meta = core.get_meta(pos)
		if meta:get_int('form_ver') < 2 then
			meta:set_int('form_ver', 2)
			meta:set_string('formspec', get_coolblock_formspec())
		end
	end
})
```

{{% comment %}} cspell:enable {{% /comment %}}
