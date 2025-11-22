---
title: Using Blender
aliases:
  - /Using_Blender
  - /models/using-blender
---

# Using Blender

3D models can be used to change the appearance of [players](/for-players/player), [nodes](/for-players/nodes), [mobs](/for-players/mobs) and other entities.

If you need to create models for your [mod](/for-players/mods), it is recommended that you use [Blender](https://www.blender.org/).

## Coordinate spaces

Recall that Luanti uses a left-handed Y-up coordinate space (X axis points "right", Z axis points "forward").

Blender uses a right-handed Z-up coordinate space, where forward is instead the positive Y axis. Make sure that your models are facing that direction. When in doubt, hit G, Y, 1 - if the model has moved forward, that means it's facing the right axis. Otherwise you'll have to rotate it.

Be sure to select left-handed Y-up in the exporter if it supports that, to avoid surprises.

## Best practices

![](/images/using-blender/Blender_LockTransforms.png)

Locking your objects can prevent accidents from happening.

- Keep your scene free from unnecessary clutter.
- Always give your objects, armature bones and other assets meaningful names.
- Lock the transformations of objects and bones that should not be moving, rotating or scaling.
- Apply all object transforms before exporting. Your armature and meshes should probably all have an identity transform, that is, zero position and rotation and 1.0 scale.
- Use only armature bones to animate the model.
- Keep your animations in Actions and use the NLA editor to arrange them in a strip for exporting.
- Keep your .blend files somewhere close to or inside the mod, but not in the ./models/ folder itself.
- Try to set up your materials and the viewport to resemble how things would look like in the game.
- Reference textures from the mod's ./textures/ folder directly instead of packing them or using different files. This will make it easier to keep your assets synced.

### Setting up a reference scene

![](/images/using-blender/Blender_LinkedFloor.png)

A reference environment can help you maintain a sense of scale when making your models.

It's a good idea to set up your scene background to resemble a portion of the game world. You can make yourself a reference scene like this and re-use it.

1.  Open a new .blend file for this purpose.
2.  Arrange some boxes to act as nodes, and map them with the textures of said nodes.
    1.  Make sure that you have a floor where the floor should be to place the mob or character on.
    2.  _Join_ meshes together to reduce clutter.
3.  Mark all the objects as unselectable and lock their transforms.
4.  Save this .blend somewhere.
5.  Whenever you make a new model, use the _File &rarr; Link_ feature to import this environment.
6.  The linked objects are still unselectable, so they should not interfere with your work.

### Blender 2.80 and later

#### Setting up the 3D view to look correctly

- Use exactly 1 viewport sample in the render properties to make the viewport look more or less how it would look in the game.
- Disable all postprocessing effects.
- In Color Management, use the Standard transform with 1.0 gamma, or a Raw view transform with 2.2 gamma. Do not use curves.
- Use an Emission shader node in the shader editor for unshaded preview.

## Create a basic mob for testing

To create a basic mob, copy an animal from [animals_modpack](https://github.com/sapier/animals_modpack)\-2.3.6. Then in the `init.lua` you only have to do a search and replace of the name of the animal, and delete any irrelevant code. You can edit speed, acceleration, etc as you wish.

Alternatively, simply register an entity that plays an animation. For example:

```lua
core.register_entity("gltf:frog", {
	initial_properties = {
		visual = "mesh",
		mesh = "gltf_frog.gltf",
		textures = {"gltf_frog.png"},
		backface_culling = false,
	},
	on_activate = function(self)
        -- Animation is 0.75 seconds long and plays at normal speed
		self.object:set_animation({x = 0, y = 0.75}, 1)
	end
})
```

## Making a mesh

1.  In the 3D View, use the object mode (dropdown in 3D viewport)
2.  Add objects (eg 3D Menu → Add → Mesh → Cube)
3.  Move around by right-clicking to select and dragging arrows.  
    **B** box/border select. **G** grab to move.  
    Resize or rotate using the properties viewport (has a row of camera, cube, spanner, etc at top)
4.  Select the cube
5.  Use rotation and scale to transform objects  
    \[See the hundreds of Blender tutorials for more advanced editing techniques.\]
6.  Create and transform cubes until you have the shape that you want
7.  Select all the cubes and _Join_ them into one object
8.  Apply the resulting object's transform and lock it. Your model is now ready.

## Saving your work

1.  Save as a Blender file (Ctrl-S or File → Save)
2.  Save with the name: yourname.blend (or similar)

## Texturing

When the basic model is completed, you'll need to create a texture.

1.  Switch to Edit Mode
    1.  Mesh/UV Unwrap/Smart UV Project \[see one of the many UV Mapping tutorials if you want seams to match, etc\]  
        \[optional\] Set island margin to 0.004 (this leaves a gap between faces, so there will be less risk of bleeding of colour across sharp edges. 0.002 is approx 1 pixel for a 512 pixel image)
2.  Move mouse to the top of the 3D window to get and up/down arrow. Right click/split window and size to 50:50.
    1.  On the R hand new window select the little cube (3D viewport) icon and switch to UV Map viewport
    2.  LowMenubar:New
        1.  Set name - eg 'yourname UV Map'
        2.  Click UV Test Grid (optional)
        3.  OK (=Save)  
            \[if you want to resize the image use a power of 2 (512/1024/etc) for x and y dimensions as it significantly speeds processing\]
    3.  LowMenubar:Image → Pack as PNG → Accept warning (this ensures you will save a copy of the image within the .blend file (I think in 2.4 this may have to be updated manually, but the tickbox in 2.7 implies is should be saved with the rest of the file)
    4.  Check UV map fits on image - adjust with: **G** - move, **R** - rotate, **S** - scale (menu:UV → Transform)  
        The UV vertices won't always align to pixel boundaries, which means if you don't use 'island margin' above, then painting on one face may also unavoidable paint onto another face. There is reference in the manual to a UV snap-to-pixel option (to align UV vertices to pixel edges) but I haven't figured how to access it yet (and it would only perfectly stop bleed for horizontal or vertically aligned vertices).
3.  In 3D View window
    1.  Switch to Texture Paint (from Edit Mode)
    2.  in the Toolbar set:-
        1.  Brush  
            Select white (should be selected by default)  
            Strength 1.0 (ie 100% replacement of the underlying colour)  
            Radius 500 (or whatever brush radius you want)
        2.  Curve  
            select the square profile if you want a solid colour brush  
            select the 'normal distribution curve' if you want a fading brush  
            (or any other profile you like)
4.  UV Map window
    1.  Switch to Paint (from View)
    2.  Paint the entire object white (or some other basecoat). This is easiest to do in UV Map window
5.  In 3D View window
    1.  To paint only particular faces, click the face Menu/faceselect (the cube with grid pattern on the face), you will then be able to select an entire face/s with R click (or shift R-click for multiple faces).
    2.  Change colour, strength, radius, curve… and paint the different parts as you wish – painting in either 3D or UV Mesh windows.  
        NB the colour picker has an eyedropper to copy the colour from within the 3D or UV Map windows – just click on the currently displayed colour (under the colour wheel) to bring it up.

## Skinning

1.  Create an armature at the scene's origin.
2.  Enter _Edit Mode_ and extrude, duplicate and transform the bones to set the skeleton up.
3.  Select the mesh, then the armature, and parent. Use empty groups, or automatic weights.
    1.  If you used empty groups or need to correct the auto weights, select the mesh and switch to _Weight Paint_ mode and bind vertices to bones by selecting a vertex group and painting the vertices in the 3D view.
    2.  If you need to bind a portion of the mesh rigidly, enter _Edit Mode_ with the mesh selected, and select the vertices that you want to bind. Select the appropriate vertex group in the Properties window. Then, click Remove and then Assign with the weight slider set to 1. Now the vertices will be bound rigidly to that bone only.
    3.  Use _Linked Select_ to quickly select a contiguous region of a mesh.

## Animation

![](/images/using-blender/Blender_AnimLayout.png)

An example window layout for working with animations.

When animating, use the _Dope Sheet_ in the _Action Editor_ mode instead of the timeline.
Do not attempt to export separate actions, instead use the Nonlinear Animation (NLA) editor to arrange and mix your actions into a single strip for export.

### Making an Action

![](/images/using-blender/Blender_FakeUser.png)

Check this to avoid losing your work.

1.  Select your armature and enter the _Pose Mode_.
2.  Create a new action by hitting the _New_ button in the action editor.
3.  Rename your action to something like "Idle" or "Running".
4.  Click the _Fake User_ icon to make sure this action is not lost when you save and reload the file.
5.  Scrub to the appropriate time code (frame number) in the dopesheet.
6.  Select and position a bone or several of them.
7.  Set a keyframe. The key should appear in the dopesheet window, and a channel will be made for the bone if it doesn't already exist.
8.  Animate the rest of the bones in this way until the action looks good.

Be sure to set one keyframe for all bones at the beginning and end frames of the action, otherwise motion may "bleed" between various actions.

#### Looping an Action

![](/images/using-blender/Blender_CyclicFCurves.png)

Apply this modifier to your channels to loop them.

A naively created looping animation will visibly slow down and restart with every cycle, even if you use an identical pose for the beginning and end keys.

To make an action loop correctly, you need to add a looping F-curve modifier to all your animation curves (dopesheet channels).

1.  Select all the **channels** in the dopesheet. Make sure you're selecting channels, not keyframes - hover your cursor over the channel names and hit 'A' once or twice until they all light up.
2.  Apply the _Make Cyclic_ operator. (Use the search bar!) Your animation should now loop correctly.

### Arranging Actions in NLA tracks

![](/images/using-blender/Blender_NLA.png)

Mixing actions can save you a lot of time.

In order to place an action in a track, you first need to push it down while it is selected.

It is possible to have many tracks and mix different animations by playing them simultaneously, which can save you a lot of work. For example, you can animate hand poses separately from the usual animations and only switch between them in another track. If you want to do this, you will probably want to utilize _Keying Sets_ and _Armature Layers_ to keep bone groups separate.

### Setting the animation range

Before you export, you need to make sure that the scene's animation range encompasses all the actions that you wish to export.

You might want to set up some hotkeys so that you can easily do this without having to use the _Timeline_ or _Properties_ windows.

## Exporting

1. Save your scene
    1. Export as a `.b3d`, `.x` or `.gltf` / `.glb` file if skinned, or `.obj` if static
    2. Save as: `mymod/models/mymod_mymodel.b3d` (for example)
2. Save the texture
	1. UV Window > Image > Save As Image
	2. Save as: `mymod/textures/mymod_mymodel.png` (for example)

### Exporting `.obj`

`.obj` file support is built into Blender natively. For Luanti, you need to:

* Make sure that you check to **export the material groups**.
	* Otherwise any models with multiple materials will not texture properly inside Luanti.
    * See [this forum thread](https://forum.luanti.org/viewtopic.php?t=22990) for more details.
* Make sure to tick **Geometry > Triangulated Mesh**.
	* Models may render incorrectly if n-gons are exported.

There is no need to write the material (`.mtl`) file as Luanti will not parse it. It can be safely deleted.
However, writing it may be necessary to work around [a Blender export bug](https://projects.blender.org/blender/blender/issues/127542)
(fixed in Blender 4.4+) where material groups would not be written otherwise.

### Exporting glTF

Recommended for animated models if older client support is not needed.
Blender supports glTF export natively. Using the built-in exporter is recommended.
See [this forum thread](https://forum.luanti.org/viewtopic.php?t=31133) for discussion.
The key takeaways are:

* You should set *Animation* &rarr; *Key* &rarr; *Interpolation Mode* to *Linear*
* You need to be careful about animation frame rate. Recall that in glTF,
  keyframe timestamps are in *seconds*, not integer frames indices to be played at a set frame rate.
  The settings for an appropriate conversion may vary, but the following settings under *Output* (in the side bar) &rarr; *Format* should be most sensible:
  * *Frame Rate*: Custom
  * *FPS*: 1
  * *Base*: 1

The following export settings should be set:

* *Transform* &rarr; *+Y-Up*: Ticked
* *Animation* &rarr; *Animation Mode*: Scene
* *Optimize Animations* &rarr; *Force keeping channels for bones*: **Not** ticked
  * This is important so that no unnecessary channels are exported for bones which are not animated.
  * Luanti 5.10 would reject models with such constant (`STEP`) interpolation channels;
    Luanti 5.11 implements support, but unnecessarily channels
    are nevertheless just a (typically very minor) waste of resources.
  * After the fact, you can check if your `.gltf` models have this problem by looking for animation samplers
    with `"interpolation": "STEP"`; if these samplers are unnecessary,
    you can simply remove them along with the associated channels to fix up the model.

### Exporting `.b3d`

The recommended exporter for `.b3d` models is available at [io_scene_b3d](https://github.com/GreenXenith/io_scene_b3d) by GreenXenith. It works on Blender 2.93 and should also work on 3.x versions. Learn more at [Which tools to export animated models?](https://forum.luanti.org/viewtopic.php?f=47&t=24292).

#### Installing the exporter

1.  Download the Blender export script `B3DExport.py`
2.  Open Blender
3.  In Blender open the menu File/User Preferences/Addons. From here you can press 'Install Add-on from File', navigate to the `.py` file, and press 'Install'. Now go to File/User Preferences/Addons/Import Export/B3D (Blitz 3D) Model Exporter.
4.  Tick the box on the left to enable it
5.  Save User Settings
6.  Close Preferences Window

#### Importing

Importing `.b3d` models is troublesome. There is no known working Blender importer which preserves animations at the moment. 
Therefore make sure to also keep the source `.blend` files around in the repository alongside the model.

### Exporting `.x`

#### Blender 2.79 and earlier

Versions of Blender before 2.8 already come with an `.x` exporter capable of producing text `.x` files.

#### Blender 2.80 to 3.0

A version of the exporter updated for 2.8 API changes is hosted here: [io_scene_x](https://github.com/minetest/io_scene_x).

As of 2020 this exporter is known to work with Blender 3.0 alpha.

The `README.md` file in that repository contains instructions on how to deal with coordinate space and attachment problems.

## Create or copy the inventory texture

- Find or create a nice graphic to be the image in the inventory.
- A flat texture can be created with a 6 sided cube model (eg animal_yourname_yourname.png)
- The inventory image is animal_yourname_yourname_item.png

(Remember to respect copyright – use screenshots of your model, perhaps with the help of a “green screen” for transparency, if you want a simple free graphic. To turn off the 3D viewport grid floor open the properties menu (+ at top R of window), and untick display/grid floor).

## Rendering the model

If you want to be able to render the model (`F12`), you'll first need to do the following steps to enable the texture:

1.  In 3D edit view. Add → Lamp → Hemi. You might need to move this around to adjust lighting direction, but probably not.
2.  In properties viewpane:
3.  Select checkered box tab,
    1.  Change texture type to Image → Movie,
    2.  Click on image dropdown below this and select your UV texture image,
    3.  Under mapping dropdown change coord to UV.

Now, rendering should work.
