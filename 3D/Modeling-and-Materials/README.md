# 3D: Modeling and Materials

For a systematic import checklist and common shader/material failures, see [Material and Model Import Troubleshooting](./Material-Import-Troubleshooting.md).

## Model import settings
Inspect scale factor, mesh compression, normals/tangents, blend shapes, rig, animation, material import, and animation clip settings. Model pivots and units should be deliberate in the source tool and import pipeline.

## Material fundamentals
- Base Color/Albedo sets surface colour.
- Metallic controls metallic response in compatible shaders.
- Smoothness/roughness affects highlight sharpness; Unity shader conventions often expose smoothness rather than roughness directly.
- Normal maps add surface detail without extra geometry.
- Occlusion maps approximate small-scale ambient occlusion where supported.
- Shader and render-pipeline compatibility determine which properties work.

## UVs and textures
UV coordinates map a 2D texture to mesh surfaces. Stretching and seams often come from UV layout, texture resolution, or import settings.

## Common mistakes
Check normal-map texture type, material shader, model scale, pivot/origin, UVs, and pipeline compatibility. Built-in, URP, and HDRP materials are not automatically interchangeable.

Reference: [Model import settings](https://docs.unity3d.com/Manual/ImportingModelFiles.html)
