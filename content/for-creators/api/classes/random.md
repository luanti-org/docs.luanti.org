---
title: Random
aliases:
  - /api/classes/random
---

# Random

Luanti provides four different random sources, each with its own merits. Modders must choose wisely unless they can let the engine do the random for them (e.g. randomly picking a sound or a texture for particles).

## Lua builtins

Not restricted by mod security, these functions are available to both SSMs and CSMs:

### [`math.randomseed`](https://www.lua.org/manual/5.1/manual.html#pdf-math.randomseed)

Seed the random. Luanti already does this for you using the system time.

{{< notice info >}}
Do not seed the random to turn it into a deterministic random source as other mods may expect it to be "non-deterministic".
{{< /notice >}}

Conversely, do not rely on the random to have any particular seed either; other mods & the engine may have seeded it (using the system time) to be "non-deterministic".

The problem with `math.randomseed` is that there is only one global, hidden seed. There is no way to get the current seed out; mods can't restore their random sequence. Mods seeding the random thus necessarily conflict - unless they all expect it to be "non-deterministic" and only seed it accordingly (ideally not at all, since the engine-side seeding should suffice).

If you need `math.random` for its performance but want it to be deterministic, you may _reseed_ the random after you're done with it to ensure that it is "non-deterministic" again.

```lua
-- Use the random to generate a seed for the random; preferable over using system time,
-- as the latter may be deterministic
local seed = ... -- some fixed seed
local reseed = math.random(2^31-1)
math.randomseed(seed) -- temporarily make the random "deterministic"
-- ... do something using `math.random` ...
math.randomseed(reseed)
```

### [`math.random`](https://www.lua.org/manual/5.1/manual.html#pdf-math.random)

Get a random number. Very versatile; allows getting floats between `0` and `1` or integers in a range.

{{< notice note >}}
The random numbers between `0` and `1` do not provide a full 52-bit mantissa full of entropy; they usually have around 32 bits of entropy.
{{< /notice >}}

{{< notice warning >}}
When using this to obtain integers, make sure that both the upper & lower bound as well as their difference are within the C `int` range - otherwise you may get overflows & errors.
{{< /notice >}}

{{< notice warning >}}
This is not portable; different builds on different platforms will produce different random numbers. PUC Lua 5.1 builds use a system-provided random generator. LuaJIT builds use LuaJIT's PRNG implementation. Do not use `math.random` in mapgen, for example.
{{< /notice >}}

{{< notice tip >}}
Use `math.random` as your go-to versatile "non-deterministic" random source.
{{< /notice >}}

## Random Number Generators

### `PcgRandom`

A seedable 32-bit signed integer pseudo-random number generator.

#### `PcgRandom(seed)`

Constructs a `PcgRandom` instance with the given seed, which should be an integer within 32-bit bounds.

#### `:next([min, max])`

If `min` and `max` are both omitted, they default to `-2^31` (`-2147483648`) and `2^31 - 1` (`2147483647`) respectively.

#### `:rand_normal_dist(min, max, [num_trials])`

{{< notice warning >}}
No successful use of this function is documented. Consider implementing your own normal distribution instead.
{{< /notice >}}

`min` and `max` are required; they need to be integers.

Rough approximation of a normal distribution with a mean of `(max - min) / 2` and a variance of `(((max - min + 1) ^ 2) - 1) / (12 * num_trials)`.

`num_trials` defaults to `6`. The more trials, the better the approximation.

The return value is a float.

### `PseudoRandom`

A seedable 16-bit unsigned integer pseudo-random number generator.

"Uses a well-known LCG algorithm introduced by K&R."

Perhaps the lowest-quality random generator of all.

#### `PseudoRandom(seed)`

Constructor: Takes a `seed` and returns a `PseudoRandom` object.

#### `:next([min, max])`

If `min` and `max` are both omitted, they default to `0` and `2^16-1` (`32767`) respectively.

{{< notice warning >}}
Requires `((max - min) == 32767) or ((max-min) <= 6553))` for a proper distribution.
{{< /notice >}}

### `SecureRandom`

System-provided cryptographically secure random: An attacker should not be able to predict the generated sequence of random numbers. Use this when generating cryptographic keys or tokens.

{{< notice note >}}
On Windows, the Win32 Crypto API is used to retrieve cryptographically secure random values which is available on every supported version of Windows. On any other platform it is retrieved from `/dev/urandom` which should be available on all Unix-like platforms such as Linux and Android.
{{< /notice >}}

#### `SecureRandom()`

Constructor: Returns a SecureRandom object.

{{< notice info >}}
Previously this could return `nil` if it can't retrieve a source of randomness, but Luanti 5.10 will always return an object and throw an error on very obscure platforms where it is not available. From a modder's point of view you can rely on it always being available now.
{{< /notice >}}

#### `:next_bytes([count])`

Only argument is `count`, an optional integer defaulting to `1` and limited to `2048` specifying how many bytes are to be returned. Returned as a Lua bytestring of length `count`

## Benchmarking

```lua
collectgarbage"stop" -- we don't want GC heuristics to interfere

local n = 1e8 -- number of runs
local function bench(name, constructor, invocation)
	local func = assert(loadstring(([[
local r = %s
for _ = 1, %d do %s end
]]):format(constructor, n, invocation)))
	local t = core.get_us_time()
	func()
	print(name, (core.get_us_time() - t) / n, "µs/call")
end

bench("Lua", "nil", "math.random()")
bench("PCG", "PcgRandom(42)", "r:next()")
bench("K&R", "PseudoRandom(42)", "r:next()")
bench("Secure", "assert(SecureRandom())", "r:next_bytes()")
```

Example output:

```
Lua	0.00385002	µs/call
PCG	0.05579729	µs/call
K&R	0.05859349	µs/call
Secure	0.11211887	µs/call
```

## Comparison

| Random Source  | Performance      | Bytes of entropy | Seedability                    | Versatility    | Distribution                             | Security                     | Portability        |
| -------------- | ---------------- | ---------------- | ------------------------------ | -------------- | ---------------------------------------- | ---------------------------- | ------------------ |
| `math.random`  | very good (1x)   | up to 4          | global seed; seeded by default | very good      | no guarantees, but usually decent enough | not cryptographically secure | varies by platform |
| `PcgRandom`    | okay (~14x)      | up to 4          | per-instance seed              | very good      | good, decent guarantees                  | not cryptographically secure | always the same    |
| `PseudoRandom` | okay (~15x)      | 1 to 2           | per-instance seed              | outright sucks | okay-ish                                 | not cryptographically secure | always the same    |
| `SecureRandom` | still okay (30x) | 1 to 2048        | not seedable                   |                |                                          | cryptographically secure     | varies by platform |

Note: The performance comparison is a bit of an apples-to-oranges comparison for multiple reasons:

1. The different generators make different guarantees regarding the randomness;
1. The different generators generate different numbers of bytes per invocation - the default was arbitrarily chosen; Secure random in particular is able to generate plenty of bytes (up to 2048) with one call.

The benchmark still suffices to draw basic conclusions though, especially for the common case where a random source is simply used once (e.g. `math.random() < 0.5`).

### Conclusion

1. _Never use `PseudoRandom`. It is strictly inferior to `PcgRandom`._
1. Use `math.random` if you want a fast "non-deterministic" random.
1. Use `PcgRandom` if you need per-instance seedability and can take the performance hit.
1. Use `SecureRandom` if and only if you need a cryptographically secure random.
