# ztracks-openrails
Ztracks Profiles for OpenRails

Erick Cantu: "I have created the master track profiles based on the new revised base mesh."

Included in this repository are:

1. The 7 master track profiles (A1t, A2t, A3t, A4t, A1t with round tunnel, A1t with US-style tunnel, A2t with US-style tunnel) in GMax, 3DS Max 2022, .3ds, .fbx, and .obj format, including endcaps for the tunnels
2. The bitmap diffuse and opacity maps used in Max

I am going to start work on a layered master texture kit for the track textures. In the meantime, the mapping for the rails, base, and tunnel walls has been frozen, although the empty space to the right will be utilized for switch stands.

A few notes on the track materials: all tracks use either material ID 1 or a combination of material IDs 1 and 2. Material ID 1 is used on all profiles. It should be a BlendATexDiff shader with an OptSpecular0 lighting palette. Material ID 2 is used for the top of the rail on the A1t, A2t, A3t, and A4t profiles. It is not used on any of the tunnel profiles. It should use a TexDiff shader with an OptSpecular25 lighting palette.

On smoothing: Even flat polygons need to be smoothed or every single edge will result in a discrete texture vertex. Without smoothing, a simple square polygon (which consists of 2 triangles), instead of having 4 texture vertices, would have 6! This is a huge performance drain. The smoothing applied to these tracks is as follows:

-Smooth group 1 is used for the entire ballast/tie base
-Smooth group 2 is used for the tunnel walls
-Smooth group 3 is used for the top of the rails and the face of the round tunnel endcap
-Smooth group 4 is used for the sides of the rails

For Blender users, this means that smoothing should be applied to the whole mesh with sharp edges in the following places:

1. Where the sides of the rails meet the tops of the rails
2. Where the side of the round tunnel endcap meets the front of the round tunnel endcap
3. Where the tunnel walls meet the ballast/tie base

Notice that the base layer incorporates the ballast, ties, tie plates, and the bottom of the rail to create a 3D rail effect with very basic shapes. Notice also that the rail on this layer has a slightly-narrowed copy of the the railhead on it. This is because the top LOD should have no rails at all.

About LODs: I think we can more or less keep the same basic LOD setup and distances as the default track. I tend to aim for halving the detail at double the LOD distance, so my thoughts are that LODs should probably be set up like this:

500m: The basic track profile
1000m: Delete the sides of the rails
2000m: Delete the tops of the rails
4000m: Reduce the ballast profile to a single square polygon. This should be the width of the original ballast layer, set at the height of the top of the original ballast layer

In practice, the last step is easily accomplished by raising the edges of the ballast to the top level, target welding the verts of the center section to the outer vertices of the now-raised edges, and then using Unwrap UVW (or the Blender equivalent) to remap the resulting polygons to the correct width on the map.

 The end result should look like this:

![image 0](IMG/00.JPG?raw=true "Image1")
![image 1](IMG/01.JPG?raw=true "Image2")


Needed:   Other formats - glTF and Blend files.
