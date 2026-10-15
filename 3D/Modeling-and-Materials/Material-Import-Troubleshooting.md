# Materials and Model Import Troubleshooting

## Model scale and orientation
Check source units, import Scale Factor, pivot/origin, and forward/up axes. Prefer fixing a consistent asset pipeline rather than applying arbitrary scales to many scene instances.

## Materials
A material uses a shader and values that control rendering. The available properties depend on the shader and render pipeline. Built-in shaders, URP shaders, and HDRP shaders are not interchangeable by name alone.

## Texture maps
- Base Color/Albedo: surface colour.
- Normal map: small-scale surface direction detail.
- Metallic and smoothness: surface reflectance response.
- Occlusion: approximate local occlusion.
- Emission: surfaces that appear self-lit; emission does not necessarily illuminate nearby objects without additional lighting/baking features.

## Import checklist
1. Select the model asset and inspect Model, Rig, Animation, and Materials tabs.
2. Confirm scale, normals, tangents, UVs, and rig type.
3. Confirm textures use the correct texture type; normal maps need the correct import setting.
4. Confirm materials use a shader compatible with the active render pipeline.
5. Inspect Console warnings after reimporting.
6. Compare the asset in a neutral test scene with one light and a known material.

## Common problems
- **Pink material:** often a missing or incompatible shader or render-pipeline conversion issue.
- **Black/odd shading:** check normals, material values, lighting, and shader support.
- **Wrong size:** check source units and Scale Factor.
- **Missing texture:** inspect material slots and texture references.
- **Unexpected transparency:** inspect surface type, alpha clipping/blending, and texture alpha.

References: [Model import settings](https://docs.unity3d.com/Manual/ImportingModelFiles.html) · [Materials](https://docs.unity3d.com/Manual/Materials.html)
