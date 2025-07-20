---
title: Models
aliases:
  - /models
bookCollapseSection: true
---

# Models

Luanti supports a few different model formats. An overview including recommendations is given below.

See ["Using Blender"](/for-creators/models/using-blender) and ["Using Blockbench"](/for-creators/models/blockbench) for information on how to create models.

## General considerations

### Coordinate systems

Luanti uses a left-handed Y-up coordinate system. That means:

- Z axis points "forward"
- Y axis points "up"
- X axis points "right"

Model editors or file formats may prescribe different coordinate systems, e.g. right-handed.
Typically, everything should work out of the box if exporters and Luanti do their conversions correctly
(which may however require configuring exporters).

See [Scratchapixel](https://www.scratchapixel.com/lessons/mathematics-physics-for-computer-graphics/geometry/coordinate-systems.html)
for a more in-depth explanation and some nice pictures to help you visualize this.

#### Scaling

For legacy reasons, entities always use a 10x scale: 10 mesh units = 1 node.

For nodes, the situation is unfortunately more complicated and depends on the file:

* For glTF models: 10 units = 1 node (consistent with the scale for entities).
* For `.obj` models: 1 unit = 1 node.
* For `.b3d` and `.x` models: 1 unit = 1 node if static, otherwise 10 units = 1 node.

If you use only glTF models, all you have to remember is that models use 10x scale.
If you use `.obj` models for nodes, you additionally have to remember that they use 1:1 scale.

{{< notice tip >}}
You can use the `visual_scale` multiplier for nodes to achieve the expected scale.
{{< /notice >}}

### Vertex format

Luanti currently uses the following vertex attributes:

* Vertex position
* Vertex normals
* One texture coordinate channel

### Animation

Animation is currently only supported in the form of playing ranges on a single timeline.
Only a single range can be played at a time (animations can not be "combined" yet).
Keyframes are interpolated linearly, though glTF also supports constant interpolation.

We distinguish *skeletal* and *rigid* animation:
In rigid animation, nodes (meshes or parents of meshes) in the models node hierarchy can be animated;
in skeletal animation, bones can be animated, and multiple bones may influence vertices via weights.
Rigid animation can be achieved as a special case of skeletal animation by having a bone to which all vertices of a mesh are attached with weight 1.

Nodes do not support animations; entities and the `model[]` formspec element do.

### Materials and textures

Multiple materials are allowed on all mesh formats.
You can assign different textures to each material slot in the Lua API:

* For nodes, use the `tiles` property in the node definition (limited to 6 materials)
* For entities, use the `textures` object property
* For the `model[]` formspec element, use the appropriate parameter

Use only one UV coordinate set for your models; do not use vertex colors.

## File formats

### `.obj` format

Simple text-based model file format; does not support animations.
Very easy to read or edit manually or programmatically.
Supported by practically every model editor.

Recommended for static models.

{{< notice tip >}}
`.obj` files may be optimized by stripping leading or trailing zeroes and removing comments (lossless) or truncating digits (lossy). See [here](https://github.com/ExeVirus/Compress-Obj) for a tool that does this.
{{< /notice >}}

The precise subset supported by Luanti is specified [here](/for-creators/models/obj-spec).

### glTF format

Modern animated model file format with decent software support (e.g. supported natively by Blender and Blockbench).
Requires Luanti 5.10 or newer clients.

Hybrid format: `.gltf` is a JSON file with binary buffers embedded in base64-encoded form;
`.glb` is a JSON file embedded in a binary container, followed by a binary buffer.

Due to being just a JSON file, `.gltf` is generally easy to edit or inspect by hand,
as long as you do not need to edit vertex or animation data, which is contained in the binary buffers.

Luanti's glTF support is currently limited to a subset documented in [`lua_api.md`](https://api.luanti.org/mods/#gltf), which already exceeds the capabilities of other model file formats and is actively being extended.

If you can tolerate not supporting older clients,
using glTF, especially for animated models, is strongly recommended.

{{< notice tip >}}
If you're a VS Code user, you can use the [glTF tools extension](https://marketplace.visualstudio.com/items?itemName=cesium.gltf-vscode)
to help you debug models in the editor. Among quite a few other useful features, this extension serves as glTF validator frontend.
{{< /notice >}}

{{< notice tip >}}
If a model does not work as expected, you can use Khronos' official [glTF validator](https://github.khronos.org/glTF-Validator/)
to check it for [glTF specification](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html) compliance.
{{< /notice >}}

{{< notice tip >}}
Luanti does not support embedded images in glTF models. You can use [gltfutil](https://github.com/luanti-org/modtools/blob/main/gltfutil.py) to extract or strip textures.
{{< /notice >}}

{{< notice note >}}
In contrast to `.b3d` and `.x`, glTF animation speeds are typically in seconds rather than frames.
When exporting, make sure to use the correct frame rate setting.
When using glTF models in Luanti, you will normally want to use a speed of `1`.
{{< /notice >}}

### `.b3d` format

Relatively simple old binary model file format.
Used to be the go-to format for animated models in Luanti until glTF support was introduced.

Model editor support is fairly poor; achieving a working export can be cumbersome.
As importing models is even more troublesome, keeping source files (e.g. `.blend` files) around is highly recommended.

If possible, prefer glTF over `.b3d`.

A nicely formatted copy of the specification is available [here](/for-creators/models/b3d-spec).
Luanti supports most of it; however "brush" properties except for the texture are currently effectively ignored, and animation must be skeletal - rigid animation is not supported.

### `.x` format

Old DirectX model file format with animation support; can be either in binary or text form.
Compression is not supported.

Even worse tool support than glTF, used relatively rarely (in comparison to `.b3d`) in the Luanti community.
For this reason, using `.x` is discouraged: Prefer glTF or `.b3d` if possible.

{{< notice tip >}}
*Avoid using the binary `.x` format!* It's actually just a tokenized version of the ASCII representation, and may actually be less efficient than a sufficiently optimized text `.x` file!
{{< /notice >}}

You can find the specification [here](https://web.archive.org/web/20250219090028/https://www.cgdev.net/axe/x-file.html).
