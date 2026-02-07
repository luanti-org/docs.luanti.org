---
title: Your first mob
---

# Your first mob

This guide walks you through creating a simple mob.

It covers basics of using entities, the go-to tool for any in-world objects that need to be more "dynamic" than a node, i.e. moving.

This guide assumes that you have a minimal mod `mymod` set up,
and want to edit `mymod/init.lua` to register a mob, the "bloblet".
When you're done, you will have a simple mob that attacks players.
It will look something like this:

![A pair of bloblets chasing Dev Test](bloblets.png)

## Entity definition

Entities are defined using `core.register_entity`. The absolute minimum is:

```lua
core.register_entity("mymod:bloblet", {})
```

After you've done `/giveme give` to grant yourself the privilege to spawn entities,
you can now spawn this entity with `/spawnentity mymod:bloblet`.

But this is not very helpful: You will be greeted by a "no texture" sprite.

## Visuals

We need to define some _object properties_ first.
These go in a field called `initial_properties` in the entity definition.

We opt for a `cube` visual. You could also use a model here.
By default, this is a node-sized cube. We can make it a bit smaller using `visual_size`.

The `cube` visual demands that six `textures` be supplied for all six faces of the cube.
To keep this tutorial code-only, we use [texture modifiers](/for-creators/api/texture-modifiers/).
We use green pixels for the sides, and a red pixel for the "front" (+Z) face texture.

```lua
core.register_entity("mymod:bloblet", {
	initial_properties = {
		visual = "cube",
		visual_size = vector.new(0.5, 0.5, 0.5), -- half the size along all axes
		textures = {
			-- top and bottom should be green
			"[fill:1x1:green", -- +Y
			"[fill:1x1:green", -- -Y
			-- sides too
			"[fill:1x1:green", -- +X
			"[fill:1x1:green", -- -X
			-- but not the front
			"[fill:1x1:red", -- +Z ("front")
			"[fill:1x1:green", -- -Z ("back")
		},
	},
})
```

Much better! Now we have our angry little cubes.

## Basic physics

You may notice that these cubes are not doing anything, not even falling.

We need to tell Luanti to apply gravity to them. Gravity is just downwards acceleration:
The downwards velocity should increase by about 10 meters per second squared, the gravitational constant for earth.
As a vector, this is an acceleration of `vector.new(0, -10, 0)`.

We also need to opt in to collisions to begin with.
For this we set `physical = true` and an appropriate collision box.

We do this by setting gravity once the entity has been activated.
Add the following to the entity definition:

```lua
core.register_entity("mymod:bloblet", {
	initial_properties = {
		...,
		physical = true,
		-- format is min x, y, z - max x, y, z
		-- the box extends from the object center position
		collisionbox = {-0.25, -0.25, -0.25, 0.25, 0.25, 0.25},
	},
	on_activate = function(self)
		-- self is the Lua entity, self.object is the ObjectRef
		self.object:set_acceleration(vector.new(0, -10, 0)) -- apply gravity
	end,
})
```

And now they fall!

We need to be a bit careful with our terminology from here on.
The entity instances are tandems of one Lua-side handle,
aptly called the "Lua entity" (this is what `self` in all entity callbacks is),
and a reference to a C++-side object, the "ObjectRef", accessible via `self.object`.

Custom per-instance properties we want to add go into the Lua entity `self`,
everything the engine needs to be made aware of happens through `self.object`.

## Punchable bloblets

We will want to make these bloblets attack the player. But then the player should be able to fight back.
So we should decide on "health points" (HP for short) for the bloblets. We will make bloblets very easy to kill.
We also need to adjust the selection box to match their size.

So add the following initial properties:

```lua
core.register_entity("mymod:bloblet", {
	initial_properties = {
		...,
		hp_max = 5, -- 5 health points = 2.5 hearts
		-- same as the collision box
		selectionbox = {-0.25, -0.25, -0.25, 0.25, 0.25, 0.25},
	},
	...
})
```

## Basic behavior

### Chasing

We still need the bloblets to chase the player. This will require some logic.
Bloblets need to choose a target and then move towards it.

For this tutorial, we keep the behavior very simple.
Every step (`on_step`), each bloblet looks for the closest player, and then moves in that direction.
We can use `self.object:set_velocity` for that.
We will want to make sure to preserve the Y component (affected mostly by gravity)
of the velocity so bloblets don't start floating.

```lua
core.register_entity("mymod:bloblet", {
	...,
	on_step = function(self, dtime) -- every step, for every bloblet...
		-- choose a target player
		local obj_pos = self.object:get_pos()
		local min_distance = 20 -- only consider players that are at least this close
		local target
		-- for each player...
		for _, player in ipairs(core.get_connected_players()) do
			local player_pos = player:get_pos()
			local distance = player_pos:distance(obj_pos)
			if distance < min_distance then -- is player close enough?
				min_distance = distance -- we're interested in the closest player
				target = player -- remember the target
			end
		end
		if target then -- if we have a player in range
			-- compute the direction from self to target in the x-z-plane
			local diff = target:get_pos() - obj_pos
			diff.y = 0
			-- normalize to length 1
			local movement_dir = diff:normalize()
			local movement_speed = 2 -- in meters / second
			local velocity = movement_speed * movement_dir -- in the x-z-plane
			velocity.y = self.object:get_velocity().y -- preserve vertical velocity due to falling
			self.object:set_velocity(velocity)
		end
	end,
})
```

But there are still more things to do.

### Jumping

The bloblet can not jump yet.
Luckily there is an easy engine solution for this: The `stepheight` object property
lets objects automatically ascend collision boxes up to a certain height.

Let's set a step height of slightly above `1` so our bloblets can jump up nodes.

```lua
core.register_entity("mymod:bloblet", {
	initial_properties = {
		...,
		stepheight = 1.01,
	},
	...
})
```

### Facing

As far as visuals go, we want the mob to face the player with its dangerously-looking red front face.

For this, we need to set a rotation every step. Edit `on_step` and add the following at the end,
inside the `if target`:

```lua
core.register_entity("mymod:bloblet", {
	...,
	on_step = function(self, dtime) -- every step, for every bloblet...
		...
		if target then -- if we have a player in range
			...
			-- rotate object appropriately. `dir:dir_to_rotation()` produces a rotation
			-- that rotates a +Z-facing object such that it points in the given direction `dir` afterwards.
			self.object:set_rotation(vector.new(diff.x, 0, diff.z):dir_to_rotation())
		end
	end,
})
```

We use `vector.dir_to_rotation` here to figure out the appropriate rotation. [^atan2]

[^atan2]:
    Because this is just in the X-Z-plane, setting a yaw of `-math.atan2(diff.x, diff.z)` would work equally well.
    But `vector.dir_to_rotation` is easier to understand and more flexible;
    it will come in handy if you need more than just rotation around a single axis.

### Attacking

To be dangerous, our mobs should be able to hurt the player.
We can implement this by maintaining a timer that records how much time has passed since the last attack.
This timer goes in the Lua entity, `self`. To help distinguish it from fields used by the engine,
it is good measure to prefix it with an underscore. We call our custom field `self._attack_timer`.

We initialize the timer to zero inside `on_activate`;
we "charge" inside `on_step`, by adding the `dtime`

As soon as the entity has "charged up" its attack, and come close enough, it punches the target player.
Again these are some modifications to `on_step`. However, we also need to initialize the timer in `on_activate`.

```lua
core.register_entity("mymod:bloblet", {
	...,
	on_activate = function(self)
		...
		self._attack_timer = 0 -- time since last attack in seconds
	end,
	on_step = function(self, dtime) -- every step, for every bloblet...
		...
		-- "charge attack": add step time (dtime) in seconds
		self._attack_timer = self._attack_timer + dtime
		if target then -- if we have a player in range
			...
			-- attack if charged (needs 2 seconds) and close enough (max 1 meter)
			if self._attack_timer >= 2 and min_distance <= 1 then
				target:set_hp(target:get_hp() - 1) -- do one damage
				self._attack_timer = 0 -- reset timer
			end
		end
	end,
})
```

To use the engine's damage system, we could ideally use `target:punch(...)`
with the appropriate tool capabilities and damage groups.
But to keep things simple, we use `set_hp` for now.

## Spawning

So far, we have had to spawn the bloblets ourselves.
In a game, you want them to just appear. We can roll some simple spawning logic.

The crucial Luanti API functions to use here are `core.raycast` to find a suitable spawn position,
and `core.add_entity` to actually spawn the entity.

```lua
local spawn_timer = 0
core.register_globalstep(function(dtime) -- as in on_step, dtime is time that has passed in seconds
	spawn_timer = spawn_timer + dtime
	if spawn_timer >= 10 then -- every 10 seconds, try to spawn a new bloblet
		spawn_timer = 0
		local players = core.get_connected_players()
		-- pick a random player
		local player = players[math.random(1, #players)]
		for _ = 1, 100 do -- cap the number of tries
			-- random point up to 25 nodes away in each direction, and 10 nodes higher.
			-- we want to raycast down from here to find a suitable spawn position.
			local start = player:get_pos() + vector.new(math.random(-25, 25), 10, math.random(-25, 25))
			if start:distance(player:get_pos()) >= 10 then -- don't spawn too close
				-- raycast down to find a node to spawn on, if any
				local thing = core.raycast(start, start + vector.new(0, -10, 0), false):next()
				if thing then
					core.add_entity(thing.above, "mymod:bloblet") -- spawn bloblet on node
					break
				end
			end
		end
	end
end)
```

## The complete code

It's okay if you don't understand everything in full detail at this point.
Just mess with the code a bit, dig into the [docs](https://docs.luanti.org/for-creators/) and the [API reference](https://api.luanti.org/).

Here's the full working code for `mymod/init.lua`:

```lua
core.register_entity("mymod:bloblet", {
	initial_properties = {
		visual = "cube",
		visual_size = vector.new(0.5, 0.5, 0.5), -- half the size along all axes
		textures = {
			-- top and bottom should be green
			"[fill:1x1:green", -- +Y
			"[fill:1x1:green", -- -Y
			-- sides too
			"[fill:1x1:green", -- +X
			"[fill:1x1:green", -- -X
			-- but not the front
			"[fill:1x1:red", -- +Z ("front")
			"[fill:1x1:green", -- -Z ("back")
		},
		physical = true,
		-- format is min x, y, z - max x, y, z
		-- the box extends from the object center position
		collisionbox = {-0.25, -0.25, -0.25, 0.25, 0.25, 0.25},
		-- set up properties so the bloblet can be punched and killed easily
		hp_max = 5, -- 5 health points = 2.5 hearts
		-- same as the collision box
		selectionbox = {-0.25, -0.25, -0.25, 0.25, 0.25, 0.25},
		-- automatically step up nodes
		stepheight = 1,
	},
	on_activate = function(self)
		-- self is the Lua entity, self.object is the ObjectRef
		self.object:set_acceleration(vector.new(0, -10, 0)) -- apply gravity
		self._attack_timer = 0 -- time since last attack in seconds
	end,
	on_step = function(self, dtime) -- every step, for every bloblet...
		-- choose a target player
		local obj_pos = self.object:get_pos()
		local min_distance = 20 -- only consider players that are at least this close
		local target
		-- for each player...
		for _, player in ipairs(core.get_connected_players()) do
			local player_pos = player:get_pos()
			local distance = player_pos:distance(obj_pos)
			if distance < min_distance then -- is player close enough?
				min_distance = distance -- we're interested in the closest player
				target = player -- remember the target
			end
		end

		-- "charge attack": add step time (dtime) in seconds
		self._attack_timer = self._attack_timer + dtime
		if target then -- if we have a player in range
			-- compute the direction from self to target in the x-z-plane
			local diff = target:get_pos() - obj_pos
			diff.y = 0
			-- normalize to length 1
			local movement_dir = diff:normalize()
			local movement_speed = 2 -- in meters / second
			local velocity = movement_speed * movement_dir -- in the x-z-plane
			velocity.y = self.object:get_velocity().y -- preserve vertical velocity due to falling
			self.object:set_velocity(velocity)

			-- rotate object appropriately. `dir:dir_to_rotation()` produces a rotation
			-- that rotates a +Z-facing object such that it points in the given direction `dir` afterwards.
			self.object:set_rotation(vector.new(diff.x, 0, diff.z):dir_to_rotation())

			-- attack if charged (needs 2 seconds) and close enough (max 1 meter)
			if self._attack_timer >= 2 and min_distance <= 1 then
				target:set_hp(target:get_hp() - 1) -- do one damage
				self._attack_timer = 0 -- reset timer
			end
		end
	end,
})

-- Spawning logic
local spawn_timer = 0
core.register_globalstep(function(dtime) -- as in on_step, dtime is time that has passed in seconds
	spawn_timer = spawn_timer + dtime
	if spawn_timer >= 10 then -- every 10 seconds, try to spawn a new bloblet
		spawn_timer = 0
		local players = core.get_connected_players()
		-- pick a random player
		local player = players[math.random(1, #players)]
		for _ = 1, 100 do -- cap the number of tries
			-- random point up to 25 nodes away in each direction, and 10 nodes higher.
			-- we want to raycast down from here to find a suitable spawn position.
			local start = player:get_pos() + vector.new(math.random(-25, 25), 10, math.random(-25, 25))
			if start:distance(player:get_pos()) >= 10 then -- don't spawn too close
				-- raycast down to find a node to spawn on, if any
				local thing = core.raycast(start, start + vector.new(0, -10, 0), false):next()
				if thing then
					core.add_entity(thing.above, "mymod:bloblet") -- spawn bloblet on node
					break
				end
			end
		end
	end
end)
```

## Next steps

A lot more is needed for a serious mob mod. Here are some things you could do:

- Draw proper textures for the front, back, sides, top and bottom faces, and use them.
- Extend the targeting logic to include other non-player entities.
  - Add "good" bloblets that fight bad bloblets.
- Don't want to reinvent the wheel? Look for mob library mods on [ContentDB](https://content.luanti.org/)
  and try to use them to implement a mob.
- Contemplate and tweak the spawning logic.
- Use a particle spawner for an attack animation.
- Look into how to use a proper model, perhaps even with animations.
- The current spawning logic lets objects spawn right in front of a player.
  It would be a better idea to spawn objects _behind_ players so they don't notice the spawning happening.
  This can be done using `player:get_look_dir()` and a (2d) dot project, or another angle computation.
- Implement more sophisticated behavior. For example, let bloblets roam the world,
  moving in random directions, when they don't have a target.
