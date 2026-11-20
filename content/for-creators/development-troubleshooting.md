---
title: Development troubleshooting
aliases:
  - /Development_troubleshooting
  - /development-troubleshooting
  - /content-dev/development-troubleshooting
---

## Lua API

Be sure to check out the [Lua 5.1 Reference Manual](http://www.lua.org/manual/5.1/manual.html) and actually understand the Lua functions you are using well, in order to minimize stupid mistakes.

Q. I've created a new class for the Lua API, but whenever I try to call a method from it, I get a Lua error that says something along the lines of "attempt to index local 'objecthere' (a userdata value)"
A. Did you forget to register the class? An object from C is Lua userdata unless it has an \_\_index and \_\_metatable field describing that it is a class and what members it has. See the static Register() method from the LuaPerlinNoise as an example of what to do. Be sure you remember to call your Register function in the body of scriptapi_export().

##### Small, aggravating things about Lua

- The end condition for a loop is actually "up to and including", not "up to" like most other languages and as is nearly always used in languages that let you define the loop condition (e.g. curly bracket languages). This means that 'for i = 0, 20 do' actually executes 21 times, not 20.
- Arrays in Lua are supposed to start at 1, not 0. Even though 0 is a perfectly acceptable index and may be used in your own code, all Lua standard functions (and Luanti API functions) will start indexing at 1.

On that note...
Q. It seems the array I pass to/get from a Lua API function got corrupted somehow, that is, certain elements are nil when they shouldn't be.
A. You may be using the wrong index from within Lua. Arrays in Lua are really associative arrays/hashtables. As a result, anything may be used to index these tables - including floating point numbers. Some Lua code you're writing may look like this:

```
x1,y1,z1 = get_start_pos()
x2,y2,z2 = get_end_pos()
for z = z1, z2 - 1 do
for y = y1, y2 - 1 do
for x = x1, x2 - 1 do
    foo(array[z - z1][y - y1][x - x1])
```

Due to floating point error, z - z1 may not be the integer 0 like you expect, but rather 0.00000000000002 or something of the sort. Lua's number-to-string conversion is not exact, and you may think that your index is an integer when it is not. To solve this, access arrays like so:

```
array[math.floor(z - z1)][math.floor(y - y1)][math.floor(x - x1)]
```

## Nodedef

Q. I have some new data structure that gets initialized in the Server ctor that creates MapNodes on initialization, but I always seem to get a CONTENT_IGNORE node.
A. You need to ensure that at least the piece of code creating the MapNodes runs \_after\_ the Lua scripts are initially run, otherwise the node definitions will not be registered.

Q. I use alias node name strings to register something in via Lua on initialization, but I keep getting CONTENT_IGNORE, even though the "default:\*" node names work!
A. Unfortunately, the node aliases, although registered, are not yet present in the nodeid-to-name mapping table. The mapping table must be manually updated with CNodeDefManager::updateAliases() first. To work around this, consider storing the node names and getting the content IDs later.

## Perlin Noise

Q. All of my noise is incoherent! It looks like a bunch of diagonal lines (for example) but is for the most part, crazy!
A. Check out the noise->result array in a debugger (or print them out, whichever is more convenient). Do the values look sane?
If so, are you accessing the correct elements? A common problem is to forget to access the array with relative coordinates, but the out-of-bounds access isn't caught since your debugging mode for your compiler did not add in any run-time bounds checking, and you wouldn't notice it until you actually crash. Another potential problem is not multiplying by the correct 'stride' value (number of bytes to skip in order to get to the next row).
If not, chances are that some input is erroneous or the result\[\] array is being corrupted for whatever reason. Try setting a data write breakpoint on it to ensure that it isn't being written to somewhere else other than within Noise::perlinMap2D/perlinMap3D.

Q. My noise works, but it seems like each generated chunk doesn't match up with the others.
A. Do you have code like this?

```
int i = 0;
for (int x = x1; x != x2; x++) {
    for (int y = y1; y != y2; y++) {
        foo(noise->result[i++]);
```

If so, you have inadvertently transposed your matrix. Read the values in the correct order (i.e., `for (y ..) { ... for (x ..) { ...`)
If that's not the case, it could then be that you're either:

1. Modifying the NoiseParams structure associated with the Noise object inadvertently, or
2. Not passing the correct coordinates to Noise::getPerlinMap2D/3D(). The _absolute coordinates_ are supposed to be passed to that method - don't worry about noise spread factor; it's taken care of. That is, if you intend to get a map of the Perlin noise starting at x=100, y=200, actually pass 100, 200.
   If you wish to add an absolute offset to the position for whatever reason (\*cough\* compatibility), then you must explicitly multiply the constant by the noise spread factor.

## Lighting in Generated Chunks

Q. I just generated a chunk of nodes in a VoxelManipulator and I then calculate the lighting, but the entire world is still dark! I can't see anything unless I turn on fly mode, then the background is sky blue (and the rest is black).
A. Are you accidentally writing `CONTENT_IGNORE` underneath the 'overtop' which `voxalgo::propogateSunlight()` checks for sunlight? (Do recall that `CONTENT_IGNORE` does not allow light to pass through.)
The `VoxelManipulator` being passed to `makeChunk()` through the `BlockMakeData` structure is not _actually_ bounded by `blockpos_min` and `blockpos_max`; whatever is present in this buffer zone that is not `CONTENT_IGNORE` is blitted back to the map as well, allowing for things such as caves and what not to not necessarily be connected from chunk to chunk. Don't _accidentally_ write nodes here!

## Luanti crashes

Q. In some situation, e.g. when I enable public serverlist, Luanti crashes giving an error message. How can I find out more about what's wrong?
A. If you use Linux, you can use [gdb](https://en.wikipedia.org/wiki/GNU_Debugger).

- You need to have a Luanti compiled as debug build (`-DCMAKE_BUILD_TYPE=Debug`).

- You need to have gdb installed
- Then run $ `bin/luanti --debugger --verbose`
- After that, Luanti should start. Now reproduce the crash.
- Once Luanti crashes, a backtrace will automatically be printed

Sometimes the command $ `dmesg` can be useful, e.g. when the crash doesn't only happen in Luanti.
