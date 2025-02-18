---
title: Using Blockbench
aliases:
  - /models/blockbench
---

# Using Blockbench

Blockbench is a 3D modelling software designed for making low-poly and voxel models, making it suitable for making Luanti models. As Blockbench supports exporting to .obj it is very easy to create static models and export them directly into Luanti.

It is also possible to use Blockbench for making nodeboxes for Luanti, as well as making animated models that can be directly exported as GLTF in Luanti 5.10 and newer.

## Static models

When opening Blockbench and creating a new model it will ask you about the model type. Pick _Generic Model_, which has no limitations and allows you to export for use with Luanti:

![](/images/using_blockbench/generic_model.webp)

Once created, you are now in the editor which allows you to start modelling. Create some cubes, resize them and move them around, or create meshes where you can manipulate individual vertices.

In terms of scale and the origin, the origin in Blockbench is the same as the origin in Luanti. So e.g. for a cube model mimicking a full node, the cube would have the position of (-8,-8,-8), size of (16,16,16) and centred around the origin:

![](/images/using_blockbench/full_block.webp)

[Export your model](#exporting-a-model) into the `models`-directory of your mod.

Since Luanti does not use embedded texture information or material files, you need to specify the texture location in the code. For example, for a node with a `drawtype` of `mesh` you specify the model textures in the tiles table:

```lua
core.register_node(":test:red_block", {
    description = "Red block",        -- user-facing name in the UI
    drawtype = "mesh",
    mesh = "test_block.gltf"          -- will be searched in the `models` directory
    tiles = { "test_block_red.png" }, -- will be searched in the `textures` or `models` directory
})
```

Even if you have exported your model, **make sure to save a .bbproject project file too!** While Blockbench can import existing models from a `.gltf` or `.obj` file, some amount of data gets lost during this process. It's best to save a project file and check it into git such that you or someone else can come back to edit the model at a later date if necessary.

## Nodeboxes

Luanti has a rather basic built-in model format for nodes consisting of a list of cubes called nodeboxes.

There is no direct nodebox export option in Blockbench, but as it allows you to add, move and scale cubes it is easy to use it to visualise a nodebox and manually convert it into nodebox coordinates.

You can also use the [objtonodebox](https://github.com/regulus79/objtonodebox) Python script which can take an .obj model and convert it into a nodebox definition. Simply export your nodebox to .obj from Blockbench and run it through the `convert.py` script.

## Animated models

Blockbench supports rigging models if you select the Animate tab in the top right corner. These animations are kept when exporting to GLTF. As of 5.10 Luanti now supports GLTF models with animations, which makes it possible to export an animated model directly from Blockbench into Luanti.

<!-- TODO Insert more detailed instructions on doing animated models with Blockbench here -->

Older versions of Luanti only supported the .b3d and .x model formats for animated models. These are both very ancient model formats that Blockbench cannot export natively, so you would need to go via Blender to make animated models for Luanti as it is the only modern 3D modeling software with a functioning B3D exporter. See [Using Blender](/models/using-blender) on the Luanti Wiki for more information.

## Exporting a model

Among other options Blockbench supports exporting models as `gltf` or `obj` files, which can later be loaded by Luanti.

Models shall be exported into the `models` directory of your mod. Textures may either go into the `textures` directory or into the `models` directory - Luanti will find them in either.

While Blockbench uses an internal block size of 16, Luanti expects a different size depending on the file format.

### glTF

Luanti expects `gltf`-files to use a node size of [10.0](https://github.com/luanti-org/luanti/blob/master/src/constants.h#L61) which requires Blockbench to scale down its internal model by a factor of 1.6.

To export your model select _File_ > _Export_ > _Export glTF Model_ and enter "1.6" as _Model Export Scale_. Also make sure to uncheck _Embed Textures_ as those are not supported by Luanti and will rise a warning in the logs whenever this model is loaded.

![A dialog titled "Export Options" with "Model Export Scale" set to "1.6" and "Embed Textures" unchecked; both fields are highlighted](/images/using_blockbench/export_gltf.webp)

### .obj

Luanti expects `obj`-files to use a block size of 1.0 which requires Blockbench to scale down its internal model by a factor of 16. While this is the default behavior the scale factor can be configured under: _File_ > _Preferences_ _Settings…_ > _Export_ > _Model Export Scale_

To export your model select _File_ > _Export_ > _Export OBJ Model_

![](/images/using_blockbench/export.webp)

Once exported into your mod's `models` folder, it will also place a `.mtl`(_material_) file along with any textures next to the exported model. The `.mtl` file is usually used to map a model's textures, but Luanti does not use this file and it can be safely removed.

![](/images/using_blockbench/files.webp)
