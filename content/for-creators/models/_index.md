---
title: Models
aliases:
  - /models
bookCollapseSection: true
---

# Models

Luanti supports a few different model formats.

### `.obj` Format

Useful for static models, doesn't not support animation.

Models stored in this format are expected to use a size of 1.0 for a single node centered around (0.0, 0.0, 0.0). This means a full node extends from (-0.5, -0.5, -0.5) to (0.5, 0.5, 0.5).

### `.b3d` Format

Originally from the Blitz3D Engine, this format is widespread due to its animation support. You will need [this](https://github.com/GreenXenith/io_scene_b3d) extension to export models from blender. It is recommended that you keep the source .blend file due to importing not being supported in the extension. Using gltf/glb is preferred if you don't mind not supporting older 5.x releases.

### `.x` Format

Alternative to b3d that supports animations. Unless you have a specific reason, should use gltf/glb or b3d.

### `.gltf / .glb` Format

Modern animation model support, added in release 5.10. Using this format means you can't (easily) support versions before 5.10. See <https://forum.luanti.org/viewtopic.php?t=31133> regarding exporting from blender at this time.

Models stored in this format are expected to use a size of [10.0](https://github.com/luanti-org/luanti/blob/master/src/constants.h#L61) for a single node centered around (0.0, 0.0, 0.0). This means a full node extends from (-5.0, -5.0, -5.0) to (5.0, 5.0, 5.0).

### Node Boxes Format

{{< notice note >}}
This Format is only supported for nodes.
{{< /notice >}}

See https://api.luanti.org/nodes/#node-boxes.

## Editors

- [BlockBench](/for-creators/models/blockbench)
- [Blender](/for-creators/models/using-blender)
