---
title: Luanti `.obj` file format specification
---

# Luanti `.obj` file format specification

The `.obj` file format supported by Luanti is a subset of [Wavefront Object (`.obj`) Files](https://web.archive.org/web/20250620145323/https://paulbourke.net/dataformats/obj/).

Material (`.mtl`) files are not supported; `.obj` files can not be animated.

## Number formatting

Numbers are formatted as either floats or integers:

### Floats

An optional minus sign (`-`), followed by one or more digits,
followed by a decimal dot (`.`), followed by one or more digits.
Example: `-42.3`.

### Integers

Optional minus sign (`-`), followed by one or more digits.
Example: `-42`.

### Indices

Indexing starts at one; indices are always integers.
Negative indices (`-1` being the last element) are supported.

## Commands

`.obj` is a line-based format.
Each line contains a "command" followed by a series of parameters, separated by spaces.
Comments are done by starting a line with `#`.

Luanti supports the following commands:

* Vertex coordinates: `v <x> <y> <z>`
* Texture coordinates: `vt <u> <v>`
* Vertex normals: `vn <x> <y> <z>`
* Groups: `g <name>` (recommended) or `usemtl <name>` introduce a new group
  * Choosing unique names is recommended; avoid empty groups
  * Subsequent faces belong to a new material (separate texture / tile)
  * Texture slots / indices are determined by order of appearance

{{< notice info >}}
Irrlicht's `.obj` reader happens to be quite lax at the moment -
just looking at the first few characters needed to tell commands apart,
ignoring superfluous parameters, ignoring unsupported commands -
but you must not rely on this.
{{< /notice >}}

## Coordinate System

Normally you should not need to care about these conventions - appropriate conversions ensure that your models look the same in Luanti as in your model editor - but knowledge may be necessary (1) to choose the correct export settings or (2) when procedurally generating `.obj` models using Lua code.

`.obj` uses a right-handed coordinate system, which is converted to Luanti's left-handed coordinate system by inverting the X-axis.

Additionally, Luanti uses V-down texture coordinates,
whereas `.obj` uses V-up, hence the `v` coordinate (texture Y-coordinate) is flipped.

The center of a node or object respectively is the origin of the `.obj`.

{{< notice note >}}
For legacy reasons, entities use a 10x scale for meshes: 10 units = 1 node.
Nodes, however, use a 1 node = 1 unit scale.
This means a full node extends from (-0.5, -0.5, -0.5) to (0.5, 0.5, 0.5);
whereas a 1³ cube entity would extend from (-5, -5, -5) to (5, 5, 5).
{{< /notice >}}

{{< notice tip >}}
You can use node `visual_scale` to compensate this inconsistency,
e.g. if you want to use the same model for a node and entity.
(Using the `visual_size` object property instead is not recommended,
as it propagates to children when using attachments;
you would have to divide the child visual size by the parent's
visual size to undo this, and remember to multiply again when detaching.)
{{< /notice >}}

## Examples

### Plane

A simple textured square at z = 0.

```obj
v 0 0 0
v 0 1 0
v 1 1 0
v 1 0 0
vt 0 0
vt 0 1
vt 1 1
vt 1 0
f 1/1 2/2 3/3
f 3/3 4/4 1/1
```

### Cube

```
# A cube (for use as a node) centered at the origin; each face receives a separate texture / tile
# no care was taken to ensure "proper" texture orientation
v -0.5 -0.5 -0.5
v -0.5 -0.5 0.5
v -0.5 0.5 -0.5
v -0.5 0.5 0.5
v 0.5 -0.5 -0.5
v 0.5 -0.5 0.5
v 0.5 0.5 -0.5
v 0.5 0.5 0.5
vn -1 0 0
vn 0 -1 0
vn 0 0 -1
vn 1 0 0
vn 0 1 0
vn 0 0 1
vt 0 0
vt 1 0
vt 0 1
vt 1 1
g right
f 1/1/1 3/3/1 2/2/1 4/4/1
g bottom
f 1/1/2 5/3/2 2/2/2 6/4/2
g front
f 1/1/3 5/3/3 3/2/3 7/4/3
g left
f 5/1/4 7/3/4 2/2/4 8/4/4
g top
f 3/1/5 7/3/5 4/2/5 8/4/5
g back
f 2/1/6 6/3/6 4/2/6 8/4/6
```
