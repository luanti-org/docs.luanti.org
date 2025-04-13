---
title: Texture Modifiers
aliases:
  - /api/texture-modifiers
---

# Texture Modifiers

Texture modifiers - strings instructing Luanti to manipulate images - are, besides dynamic media, the only way to dynamically generate textures at runtime.

They can also be used to simplify mods by generating repetitive textures such as differently colored tool textures.

{{< notice info >}}
Texture modifiers are partially redundant with hardware colorization. Hardware colorization should be preferred as it is more flexible and presumably generates fewer garbage textures.
{{< /notice >}}

## Performance Issues

- All textures are generated on the CPU, not the GPU. Generation is therefore slow and may temporarily block the client thread.
- Texture modifiers are [kept in client memory forever](https://github.com/luanti-org/luanti/issues/11531).
- Cached texture modifiers are [not leveraged when generating new ones](https://github.com/luanti-org/luanti/issues/11587).
- Non-linear (at least quadratic) time complexity of the shotgun parser is possible.

Use texture modifiers mostly statically. Keep dynamically generated textures to a minimum to not fill up the client cache over time. Don't force the client to perform many expensive texture generations in a small time span if you want a smooth client experience. Do not create large, convoluted texture modifiers if possible.

## Bugs

- Large texture modifiers may [freeze the client](https://github.com/luanti-org/luanti/issues/11829).
- Out-of-bound memory reads are possible using certain texture modifiers. Example: `[lowpart:0:blank.png`.
- Out-of-bound pixels are often considered "full white" (`#FFFFFFFF`)
- Certain invalid texture modifiers may lead to client segmentation faults.
- Syntactically invalid parts of texture modifiers may silently be ignored.

## Texture Packs

Texture modifiers can be used in node & item texture overrides.

{{< notice info >}}
Texture-modifier-generated textures can not be replaced by a texture pack (except through texture overrides); only base textures can be properly replaced.
{{< /notice >}}

Usage of certain texture modifiers might require certain texture resolutions to be used by texture packs, as texture modifiers operate on pixels instead of relative units. Lower-res TPs can usually scale up but higher-res TPs will have to scale down.

{{< notice tip >}}
Use `[resize` to forcibly resize textures to your required resolution (e.g. for `[combine`) in order to be as TP-agnostic as possible.
{{< /notice >}}

## In Lua

### API

#### `core.inventorycube(top, left, right)`

The only texture modifier utility function provided.

**Arguments:**

- `top`, `left`, `right` - string: Texture modifiers for the respective node faces

**Returns:**

- Texture modifier `[inventorycube{<top>{<left>{right>` with `top`, `left`, `right` escaped according to the rules of `inventorycube` escaping.

### String Notations

Lua provides three string notations: Quoted (single or double) and long. The type of quotes doesn't really matter, as texture names shouldn't include either type of quotes anyway. Keep in mind that to encode a backslash in quoted strings, you have to escape it with another backslash: `a.png^[mask:b.png\^c.png` would be encoded as `"a.png^[mask:b.png\\^c.png"` using double quotes.

Long strings enclose their content with two pairs of square brackets (`[]`) with a matching number of equals signs (`=`) between the two brackets of each pair:

`[[...]]` and `[===[...]===]` are examples of valid long strings.

Within them, no escaping applies, which means that texture modifiers can be written down literally;
a leading square bracket such as that of a generating texture modifier is not a problem.
Example: `[[[combine:1x1:0,0=a.png]]`.

Using equals signs is never necessary since texture modifiers won't contain `[[` or `]]`.

{{< notice tip >}}
You can use metamethods in order to implement a neat, possibly OOP-ish Lua DSL that does the escaping and formatting for you.
{{< /notice >}}

## Syntax

{{< notice warning >}}
Texture modifiers are only parsed client-side, where errors lead to poor behavior (error messages in the best case, sometimes the wrong texture, crashes in the worst case). They are not validated server-side. Take additional care to ensure no syntax errors or values which cause undefined behavior.
{{< /notice >}}

{{< notice tip >}}
Use a string builder which guarantees valid texture modifiers.
{{< /notice >}}

### Base textures

Texture modifiers work on _base textures_ which are specified in a string form as media file names. See the supported file formats.

`<...>` is used to denote texture modifier arguments below, `[...]` denotes optional parts.

All ranges denoted below are inclusive (i.e. `0` to `10` contains both `0` and `10`).

### Overlaying

Overlaying is a binary operator using the exponentiation symbol (`^`).

The right-hand operand is overlaid over the left-hand operand.

Overlaying is associative: `<a>^<b>^<c>` is the same as both `(<a>^<b>)^<c>` and `<a>^(<b>^<c>)`.

Before overlaying, the texture with the lower pixel count is upscaled to the dimensions of the texture with the higher pixel count.

Alpha blending is applied correctly.

### Argument Escaping

Argument escaping uses the backslash (`\`). It is only allowed within "combining" texture modifiers.

_All characters can be escaped_; only a few (`^`, `:` & `\`) must be escaped to allow the use of texture modifiers as arguments (not base images) within combining texture modifiers.

Nested escaping is possible; escape each backslash with a backslash for this, doubling the amount of backslashes: Nesting to a depth of `n` requires `2^n` backslashes per character to be escaped.

The `inventorycube` texture modifier uses a different form of escaping for its arguments:

`^` is replaced with `&`. `core.inventorycube(top, left, right)` performs this escaping. It is not possible to nest the `inventorycube` texture modifier within itself as it uses curly braces (`{`) for separating its arguments but does not provide a way of escaping them.

Example escaping implementation:

```lua
local function escape_argument(texture_modifier)
	return texture_modifier:gsub(".", {["\\"] = "\\\\", ["^"] = "\\^", [":"] = "\\:"})
end
```

### Grouping

The operands of the overlaying operator may be enclosed within parentheses to force them being evaluated first. This is only reasonable to force evaluation of texture modifiers before an overlay operation.

Grouping can not be used to use texture modifiers within combining texture modifiers; grouping will be ignored for delimiting purposes. You must use escaping for that.

Wrong: `a^[lowpart:1:(b^c)`, right: `a^[lowpart:1:b\^c`.

Also wrong: `[combine:1x2:0,0=(a^b):0,1=(c^[multiply:red)` - the combine parsing will ignore the parentheses and misinterpret the colon `:` before `red` as a delimiter for combine.

`[combine:1x2:0,0=(a^b):0,1=(c^d)` will work, but you shouldn't rely on it.

Grouping can however be used to enclose combining texture modifiers, separating them from the containing texture modifier.

{{< notice tip >}}
Use grouping for evaluating parts of the right-hand side first, like this: `a^[multiply:green^(b^[multiply:red)`
{{< /notice >}}

### Modifiers

All texture modifiers create new textures which can be modified further and do not modify the textures they operate on.

{{< notice tip >}}
Use `string.format("%d", number)` - `("%d"):format(number)` in shorthand - to guarantee that integers are parsable. In practice, `tostring(number)` or implicit conversion to string when concatenating will work as expected.
{{< /notice >}}

#### Combining Texture Modifiers

The following texture modifiers are considered "combining", as they operate by combining multiple textures into one, taking textures other than the "base texture" as arguments:

- `mask`: Bitwise masking (takes mask)
- `lowpart`: Blitting a lower part of one texture onto another (takes foreground)
- `combine`: Combining multiple textures through blitting at pixel locations (takes multiple textures)
- `inventorycube`: Rendering an inventorycube from three provided textures

#### Base Texture Modifiers

These texture modifiers all modify a base texture `<base>`, which may in turn consist of texture modifiers.

##### `<base>^[brighten`

Interpolates 50-50 between the color of each pixel of the base texture and white.

##### `<base>^[noalpha`

Sets the alpha channel of the base texture to full (`255`).

As the red, green and blue channels aren't premultiplied with alpha in PNGs, this might reveal hidden colors of otherwise transparent portions of an image.

##### `<base>^[makealpha:<r>,<g>,<b>`

- `r`, `g`, `b` are integers ranging from `0` to `255`.

Pixels of the base texture having the exact same RGB color will have their alpha set to `0`.

As the red, green and blue channels are kept, the original color can be restored using `[noalpha` (which will however also make originally semitransparent portions of the image opaque).

##### `<base>^[opacity:<ratio>`

- `ratio` is an integer ranging from `0` to `255`

Multiplies the alpha value of each pixel of the base texture with `ratio/255` and rounds properly afterwards.

##### `<base>^[invert:<mode>`

- `mode` is a string which may contain the characters `r`, `g`, `b` and `a`.

The channels corresponding to the occurring characters (red, green, blue and alpha) will be inverted (set to `255 - value`).

##### `<base>^[transform<transforms>`

`<transforms>` is the concatenation of either numbers or names identifying transformations from the following table:

| Number | Name  | Transformation                              |
| ------ | ----- | ------------------------------------------- |
| 0      | I     | Identity (no transformation)                |
| 1      | R90   | Rotate by 90° counterclockwise              |
| 2      | R180  | Rotate by 180° counterclockwise             |
| 3      | R270  | Rotate by 270° counterclockwise             |
| 4      | FX    | Flip X (horizontally)                       |
| 5      | FXR90 | Flip X, then rotate by 90° counterclockwise |
| 6      | FY    | Flip Y (vertically)                         |
| 7      | FYR90 | Flip Y, then rotate by 90° counterclockwise |

Transformation names are case insensitive.

##### `<base>^[verticalframe:<framecount>:<frame>`

- `framecount`: Animation frame count
- `frame`: Current animation frame, 0-indexed

Result: Vertically crops the texture by dividing the base texture height through the frame count to determine the frame height. As the division is an integer division, a remaining fractional frame will be discarded.

{{< notice warning >}}
Specifying a `framecount` of `0` will trigger a floating point exception, crashing the client.
{{< /notice >}}

##### `<base>^[crack[<opacity>]:[<framecount>:]<tilecount>:<frame>`

Shorthand for overlaying a scaled frame of the crack texture, `crack_anylength.png`,
over a texture, with options for alpha and blitting on all frames.

- `opacity`: Optional. If set to `o` (modifier name becomes `cracko`) the crack will only be overlaid over fully opaque base texture regions.
- `framecount`: Optional. Vertical animation frame count of the crack texture; usually omitted.
- `tilecount`: Vertical & horizontal tile count of the base texture. The crack will be blit on each tile of the base texture. Usually `1`.
- `frame`: Current animation frame ("crack progression").

{{< notice note >}}
This always scales the crack to the size of the base texture (or the tiles of the base texture, if `tilesize` is provided).
{{< /notice >}}

##### `<base>^[sheet:<w>x<h>:<x>,<y>`

- `w`, `h`: Tile sheet dimensions (positive integers, in tiles)
- `x`, `y`: Tile position, 0-indexed (in tiles)

Retrieves the tile at position `x`, `y`.

##### `<base>^[multiply:<color>`

- `color` is a `ColorString`

Multiplies the RGB values of the base texture per pixel with the RGB values of `color`; the alpha value of `color` is ignored.

##### `<base>^[colorize:<color>[:<ratio>]`

- `color` is a `ColorString`
- `ratio` is an optional integer from `0` to `255` or the string `alpha`

Interpolates between `color` and the pixel colors of the base texture as specified by the `ratio`:

- Defaults to the alpha of `color` if omitted;
- If it is an integer from `0` (only pixel color) to `255` (only `color`), it is directly used as interpolation ratio:
  The resulting color of a pixel is `ratio` times `color` plus `(255 - ratio)` times pixel color;
- If it is the string `alpha`, the texture pixel's alpha value determines the `ratio` per pixel

##### `<base>^[mask:<texture>`

- `texture` is an escaped texture modifier

Applies _bitwise and_ (`bit.band`) to all RGBA values of `texture` and the base texture.

The dimensions of the resulting texture are determined by the base texture.

If a pixel of the base texture is out of bounds on `texture`, it is preserved.

Masking is associative and commutative if all involved textures have the same dimensions.

##### `<base>^[lowpart:<percent>:<texture>`

- `percent` is an integer from `0` to `100`
- `texture` is an escaped texture modifier

Overlays the lower `percent` part of `texture` on the base texture.

{{< notice tip >}}
Use `blank.png` as base texture if you do not want a background.
{{< /notice >}}

#### Base Texture Generators

These modifiers do not accept a base texture as they generate one from their arguments.

##### `[png:<data>`

- `data` is a base64-encoded PNG bytestring

Creates a texture from an embedded base64-encoded PNG image.

`data` can be built by combining `core.encode_base64` and `core.encode_png`:

```lua
local function embedded_png(width, height, data)
	return "[png:" .. core.encode_base64(core.encode_png(width, height, data))
end
```

{{< notice warning >}}
Not supported by Luanti 5.4 and older clients. May lead to client crashes if used in node tiles. Luanti 5.5 and newer servers will automatically prepend `blank.png^` to `[png` tiles to mitigate this.
{{< /notice >}}

{{< notice warning >}}
Do not use this for large textures. If used as an object texture, the texture modifier will get sent arbitrarily often, putting a strain on the network.
{{< /notice >}}

{{< notice tip >}}
Consider using other texture modifiers cleverly or using dynamic media instead.
{{< /notice >}}

##### `[combine:<w>x<h>:<textures>`

- `w`: Width of the resulting texture
- `h`: Height of the resulting texture
- `textures`: Colon (`:`)-separated list of locations `x`, `y` (both integer, may be negative) and escaped texture modifiers `texture` to blit in the form `<x>,<y>=<texture>`. Can be empty.

A _combining_ texture modifier accepting other texture modifiers as arguments.

Nesting `combine` is possible through escaping.

Generates a texture of dimensions `w`, `h` on which all `textures` have been blit at the specified locations. The background is black and transparent (`#00000000`).

{{< notice info >}}
Node tiles starting with `[combine` are broken on Luanti 5.4 and older clients connecting to 5.5 servers due to the aforementioned `[png` workaround accidentally being applied to `[combine` as well.
{{< /notice >}}

{{< notice tip >}}
To work around this, you can enclose your node tile texture modifiers using `[combine` in parentheses: `([combine:...)`. A generic patch which loops over node definitions and wraps matching texture modifiers [is available as well](https://gist.github.com/appgurueu/dea1d1d9d8494e9c00114d36d58c5932).
{{< /notice >}}

##### `[inventorycube{<top>{<left>{<right>`

Renders a cube with the three given textures using simple software rendering.

The resulting image will be 9 times the nearest power of 2 large enough to contain the dimensions of the largest image, clamped to a range of at least 4 and at most 64.

As a formula: `9 * max(4, min(64, 2^ceil(log_2(max(d)))))` where `d` is the set of dimensions (width & height) of all faces.

## Examples

### Cracked Nodes

- Cracked Stone: `default_stone.png^[crack:1:2`
  - Use the third crack progression (`2` if 0-indexed)...
  - ... and draw it on a stone texture consisting of `1` tile
- Cracked Lava: `default_lava_source_animated.png^[crack:1:8:2`
  - The `default_lava_source_animated.png` texture has `8` animated vertical frames...
  - ... and exactly `1` "tile" horizontally (frames are not tiles)

If in doubt, prefer an explicit `verticalframe` and overlaying over `crack`.

### Cropping

Cropping can be accomplished using the `[combine` texture modifier:

Examples (cropping `default_stone.png`, 16x16):

- Remove left & top 6px: `[combine:8x8:-8,-8=default_stone.png`
- Remove right & bottom 6px: `[combine:8x8:0,0=default_stone.png`

### Progress Bars

Combine or lowpart and rotate (the latter more TP-agnostic)

The below examples use `progress_bar.png` & `progress_bar_bg.png` from Luanti's base textures, both 256x48.

- Using nested `combine`s: `[combine:256x48:0,0=progress_bar_bg.png:0,0=[combine\:128x48\:0,0=progress_bar.png`
  - First crop the foreground to its left half (`128` of `256` pixels); escape this
  - Then create a new blank image with the progress bar resolution, blit the background and after that the cropped foreground
  - _Downside: Heavily resolution-dependent_; making it (mostly) resolution-independent requires (up)scaling textures first
- Using `lowpart` and `transform` to rotate: `progress_bar_bg.png^[transformR90^[lowpart:50:progress_bar.png\^[transformR90^[transformR270`
  - First take the background (horizontal) and rotate it by 90° counterclockwise to orient it vertically
  - Now do the same for the foreground, again to convert horizontal into vertical orientation; escape this (`\^`)
  - Use the rotated foreground as argument to `lowpart`; blit `50` % of the vertical foreground on the vertical background
  - Rotate everything by 270° to undo the vertical orientation
  - _Advantage over using `combine`_: Due to the usage of `lowpart`, this is resolution-agnostic (will work with texture packs of different resolutions)

{{< notice note >}}
For HUDs, `statbar`s should be preferred over `image`s using texture modifiers.
{{< /notice >}}
