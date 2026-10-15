# 3D: Lighting and Rendering

## Common light types
- **Directional:** approximates a distant source such as sunlight.
- **Point:** emits in all directions from a position.
- **Spot:** emits within a cone.
- **Area lights:** availability and runtime behaviour depend on the render pipeline and use case.

## Lighting workflows
Realtime lighting updates during play. Baked lighting stores results in lightmaps. Mixed lighting combines approaches according to pipeline settings. Reflection Probes capture environment information for reflective surfaces; Light Probes provide indirect-light samples for moving objects.

## Render pipeline matters
Built-in, URP, and HDRP differ in shaders, lighting features, post-processing, and configuration. Verify which pipeline is active before following a tutorial.

## Troubleshooting
Check light culling masks, object layers, materials/shaders, renderer settings, quality settings, shadow flags, and whether lighting has been baked. Realtime shadows and many overlapping lights can be expensive, so profile at target quality.

Reference: [Lighting manual](https://docs.unity3d.com/Manual/LightingOverview.html)
