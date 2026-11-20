---
title: Lua Optimization Tips
aliases:
  - /Lua_Optimization_Tips
  - /lua-optimization-tips
  - /content-dev/lua-optimization-tips
---

Remember, every second you spend in Lua is a second that the Server thread, Connection thread, and possibly one or more Emerge threads stay at a complete standstill! For this reason, intensive mods should designed with speed in mind.

**Note**: Many of these tips are specific to neither Lua nor Luanti.

## Using Script API

### Profiling

Very simple; no reason not to do so:

```lua
local t1 = core.get_us_time()
--... work here ...
print(string.format("elapsed time: %g ms", (core.get_us_time() - t1) / 1000))
```

- Compare results often to identify bottlenecks before they become harder to find

### Loop ordering

Always use z, y, x ordering unless there's a good reason not to.

- Keeps cache coherency
- Opens opportunity to use simple arithmetic to calculate indices

### Prefer local variables

- Should usually recompute variable contents instead of computing once and storing in a global variable
- For most minor computations, the benefit of local access outweighs the cost of re-computing their contents.
- Of course, the code in concern should be profiled instead of blindly following this general rule of thumb.

### Use separate variables when possible instead of tables

- Even though putting associated variables inside of tables may keep things more orderly, the cost associated with performing a key lookup on access is high compared to a local variable reference.
- Some benchmarks have shown the opposite to be true when using LuaJIT, this must be verified.

### Avoid having to re-enter the C++ side by calling API when not needed

- Although Lua is quite slow compared to native code, the amount of time spent in switching could be much greater.
- Again, the developer's discretion and profiling is needed to determine what the best course of action is.

### Array index calculation

"Convenience" functions such as VoxelArea:index() are very attractive for simplicity's sake, but should not be used if maximizing speed is desired.
Try to use simple arithmetic like addition and subtraction for calculating array indices.
To deal with gaps in between multi-dimensional arrays:

1.  Keep a 'base' index variable that has the 0th sub-index of the current outer loop's index
2.  Add the stride length added to it per iteration of the outer loop
3.  Increment a copy of that variable within the inner loop
4.  Repeat for each dimension

This way, costly multiplications in the innermost loop can be eliminated.
Sample of a very fast loop done in this manner:

```lua
local idx_z_base = initial_index_offset
for z = z0, z1 do
	local idx_y_base = idx_z_base
	for y = y0, y1 do
		local i = idx_y_base
		for x = x0, x1 do
			-- ... work here ...
			i = i + 1
		end
		idx_y_base = idx_y_base + y_stride
	end
	idx_z_base = idx_z_base + z_stride
end
```

See also: [vmanip#Tips_for_handling_indices](/vmanip#Tips_for_handling_indices)

### Benchmarking

To test how often some function is executed in a second you can use something with a syntax similar to the one of `core.after`:

```lua
local TIME = 3

local clock = core.get_us_time
local us = TIME * 1000000
local function benchmark_function(fct, ...)
	local start = clock()
	local fin = start
	local total = 0
	while fin - start < us do
		fct(...)

		total = total + 1
		fin = clock()
	end
	return total * 1000000 / (fin - start)
end

print("hello got printed " .. benchmark_function(print, "hello") .. " times a second")

--[[ result:
[…]
hello
hello
hello
hello got printed 65592.333333333 times a second
]]
```

you could also use `os.clock` instead of `core.get_us_time` (of course you then also need to remove that multiplying with 10⁶)

## Writing Script API

### When working with arrays, use lua_rawgeti()/lua_rawseti()

- These work on plain integer indexes rather than field names and perform faster.

### Prefer lua_newtable() to lua_createtable()

It has been observed that lua_newtable() is generally faster than lua_createtable() by a rather wide margin.

- This might seem counterintuitive, as lua_createtable() preallocates a known amount of memory in bulk to save steps, but it has a very negative effect on the cache for situations where the performance improvement would have otherwise made a difference.
