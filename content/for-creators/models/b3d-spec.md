---
title: Blitz3d file format specification
---

# Blitz3d file format specification

This is a reformatted version of the specification of the Blitz3D file format, version 0.01,
presumably by `marksibly@blitzbasic.co.nz`, originally available on `http://www.blitzbasic.co.nz`
(which no longer operates) and placed in the public domain.

## Introduction

The Blitz3D file format specifies a format for storing texture, brush and entity descriptions for
use with the Blitz3D programming language.

The rationale behind the creation of this format is to allow for the generation of much richer and
more complex Blitz3D scenes than is possible using established file formats - many of which do not
support key features of Blitz3D, and all of which miss out on at least some features!

A Blitz3D (`.b3d`) file is split up into a sequence of 'chunks', each of which can contain data
and/or other chunks.

Each chunk is preceded by an eight byte header:

```
char tag[4]  ; 4 byte chunk 'tag'
int length   ; 4 byte chunk length (not including *this* header!)
```

If a chunk contains both data and other chunks, the data always appears first and is of a fixed
length.

A file parser should ignore unrecognized chunks.

Blitz3D files are stored little endian (intel) style.

Many aspects of the file format are not quite a 'perfect fit' for the way Blitz3D works. This has
been done mainly to keep the file format simple, and to make life easier for the authors of third
party importers/exporters.

## Chunk Types

This lists the types of chunks that can appear in a b3d file, and the data they contain.

Color values are always in the range 0 to 1.

string (`char[]`) values are 'C' style null terminated strings.

Quaternions are used to specify general orientations. The first value is the quaternion 'w' value,
the next 3 are the quaternion 'vector'. A 'null' rotation should be specified as 1,0,0,0.

Anything that is referenced 'by index' always appears EARLIER in the file than anything that
references it.

`brush_id` references can be -1: no brush.

In the following descriptions, `{}` is used to signify 'repeating until end of chunk'. Also, a chunk
name enclosed in `[]` signifies the chunk is optional.

Here we go!

### `BB3D`

```
int version                 ; file format version: default=1
[TEXS]                      ; optional textures chunk
[BRUS]                      ; optional brushes chunk
[NODE]                      ; optional node chunk
```

The `BB3D` chunk appears first in a b3d file, and its length contains the rest of the file.

Version is in `major*100+minor` format. To check the version, just divide by `100` and compare it with
the major version your software supports, eg:

```
if file_version/100>my_version/100
   RuntimeError "Can't handle this file version!"
EndIf

if file_version Mod 100>my_version Mod 100
   ;file is a more recent version, but should still be backwardly compatible with what we can
handle!
EndIf
```

### `TEXS`

```
{
char file[]                 ; texture file name
int flags,blend             ; blitz3D TextureFLags and TextureBlend: default=1,2
float x_pos,y_pos           ; x and y position of texture: default=0,0
float x_scale,y_scale       ; x and y scale of texture: default=1,1
float rotation              ; rotation of texture (in radians): default=0
}
```

The `TEXS` chunk contains a list of all textures used in the file.

The flags field value can conditional an additional flag value of '65536'.
This is used to indicate that the texture uses secondary UV values, ala the TextureCoords command.

### `BRUS`

```
int n_texs
{
char name[]                 ; eg "WATER" - just use texture name by default
float red,green,blue,alpha  ; Blitz3D Brushcolor and Brushalpha: default=1,1,1,1
float shininess             ; Blitz3D BrushShininess: default=0
int blend,fx                ; Blitz3D Brushblend and BrushFX: default=1,0
int texture_id[n_texs]      ; textures used in brush
}
```

The `BRUS` chunk contains a list of all brushes used in the file.

### `VRTS`

```
int flags                   ; 1=normal values present, 2=rgba values present
int tex_coord_sets          ; texture coords per vertex (eg: 1 for simple U/V) max=8
int tex_coord_set_size      ; components per set (eg: 2 for simple U/V) max=4
{
float x,y,z                 ; always present
float nx,ny,nz              ; vertex normal: present if (flags&1)
float red,green,blue,alpha  ; vertex color: present if (flags&2)
float tex_coords[tex_coord_sets][tex_coord_set_size]	; tex coords
}
```

The `VRTS` chunk contains a list of vertices. The `flags` value is used to indicate how much extra
data (normal/color) is stored with each vertex, and the `tex_coord_sets` and `tex_coord_set_size`
values describe texture coordinate information stored with each vertex.

### `TRIS`

```
int brush_id                ; brush applied to these TRIs: default=-1
{
int vertex_id[3]            ; vertex indices
}
```

The `TRIS` chunk contains a list of triangles that all share a common brush.

### `MESH`

```
int brush_id                ; 'master' brush: default=-1
VRTS                        ; vertices
TRIS[,TRIS...]              ; 1 or more sets of triangles
```

The `MESH` chunk describes a mesh. A mesh only has one `VRTS` chunk, but potentially many `TRIS` chunks.

### `BONE`

```
{
int vertex_id               ; vertex affected by this bone
float weight                ; how much the vertex is affected
}
```

The `BONE` chunk describes a bone. Weights are applied to the mesh described in the enclosing `ANIM` -
in 99% of cases, this will simply be the `MESH` contained in the root `NODE` chunk.

### `KEYS`

```
int flags                   ; 1=position, 2=scale, 4=rotation
{
int frame                   ; where key occurs
float position[3]           ; present if (flags&1)
float scale[3]              ; present if (flags&2)
float rotation[4]           ; present if (flags&4)
}
```

The `KEYS` chunk is a list of animation keys. The `flags` value describes what kind of animation
info is stored in the chunk - position, scale, rotation, or any combination of.

### `ANIM`

```
int flags                   ; unused: default=0
int frames                  ; how many frames in anim
float fps                   ; default=60
```

The `ANIM` chunk describes an animation.

### `NODE`

```
char name[]                 ; name of node
float position[3]           ; local...
float scale[3]              ; coord...
float rotation[4]           ; system...
[MESH|BONE]                 ; what 'kind' of node this is - if unrecognized, just use a Blitz3D
pivot.
[KEYS[,KEYS...]]            ; optional animation keys
[NODE[,NODE...]]            ; optional child nodes
[ANIM]                      ; optional animation
```

The `NODE` chunk describes a Blitz3D Entity. The scene hierarchy is expressed by the nesting of `NODE`
chunks.

`NODE` kinds are currently mutually exclusive - ie: a node can be a `MESH`, or a `BONE`, but not both!
However, it can be neither... if no kind is specified, the node is just a 'null' node - in Blitz3D
speak, a pivot.

The presence of an `ANIM` chunk in a `NODE` indicates that an animation starts here in the hierarchy.
This allows animations of differing speeds/lengths to be potentially nested.

There are many more 'kind' chunks coming, including camera, light, sprite, plane etc. For now, the
use of a Pivot in cases where the node kind is unknown will allow for backward compatibility.

## Examples

A typical b3d file will contain 1 `TEXS` chunk, 1 `BRUS` chunk and 1 `NODE` chunk, like this:

```
BB3D
  1
  TEXS
    ...list of textures...
  BRUS
    ...list of brushes...
  NODE
    ...stuff in the node...
```

A simple, non-animating, non-textured etc mesh might look like this:

```
BB3D
  1                           ; version
  NODE
    "root_node"               ; node name
    0,0,0                     ; position
    1,1,1                     ; scale
    1,0,0,0                   ; rotation
    MESH                      ; the mesh
      -1                      ; brush: no brush
      VRTS                    ; vertices in the mesh
        0                     ; no normal/color info in vertices
        0,0                   ; no texture coords in vertices
        {x,y,z...}            ; vertex coordinates
      TRIS                    ; triangles in the mesh
        -1                    ; no brush for this triangle
        {v0,v1,v2...}         ; vertices
```

A more complex 'skinned mesh' might look like this (only chunks shown):

```
BB3D
  TEXS                        ; texture list
  BRUS                        ; brush list
  NODE                        ; root node
    MESH                      ; mesh - the 'skin'
    ANIM                      ; anim
    NODE                      ; first child of root node -  eg: "pelvis"
      BONE                    ; vertex weights for pelvis
      KEYS                    ; anim keys for pelvis
      NODE                    ; first child of pelvis - eg: "left-thigh"
        BONE                  ; bone
        KEYS                  ; anim keys for left-thigh
      NODE                    ; second child of pelvis - eg: "right-thigh"
        BONE                  ; vertex weights for right-thigh
        KEYS                  ; anim keys for right-thigh
```

...and so on.
