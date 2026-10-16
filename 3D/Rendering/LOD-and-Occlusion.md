# LOD and occlusion culling

## Level of Detail (LOD)

LOD Groups switch between versions of a model based on screen-relative size. Use detailed meshes nearby and simpler meshes at distance. Configure transitions in the LOD Group Inspector and test at the intended camera distances.

## Occlusion culling

Occlusion culling avoids rendering objects hidden behind other geometry. It works best in scenes with substantial occluding walls and predictable spaces. Open outdoor scenes may see less benefit.

## Workflow

1. Identify large static occluders and objects likely to be hidden.
2. Mark appropriate geometry static for occlusion baking.
3. Open the Occlusion Culling window and bake with representative camera positions.
4. Test the result using the Scene view visualization and target build.
5. Profile before and after to confirm the cost/benefit.

## Other useful tools

- Frustum culling excludes objects outside the camera view automatically.
- GPU instancing can reduce draw overhead for repeated compatible meshes/materials.
- Batching and render-pipeline features depend on shader/material compatibility.
- Texture and shadow settings often matter more than adding culling indiscriminately.

## Pitfalls

LOD models need matching pivots and comparable silhouettes. Overly aggressive transitions can pop. Incorrect static flags can cause confusing bake results. Always measure with a representative scene and target hardware.
