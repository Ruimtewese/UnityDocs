# 3D: Rendering and Materials

## Guides
- [Lighting, materials, and URP](./Lighting-Materials-and-URP.md)
- [Camera and post-processing](./Post-Processing-and-Camera.md)
- [Model import settings](../Modeling-and-Materials/Model-Import-Settings.md)
- [Material import troubleshooting](../Modeling-and-Materials/Material-Import-Troubleshooting.md)

## Core topics
Materials select shaders and shader properties. Render pipeline choice (Built-in, URP, HDRP) affects shader compatibility and rendering features. Cameras control projection, field of view/orthographic size, clip planes, culling masks, and target display. Lights and shadows should match the visual goal and target performance budget.

## Pink materials
Pink/magenta commonly indicates a missing/incompatible shader or failed shader compilation. Check the active render pipeline, shader assignment, pipeline asset settings, material conversion, and custom shader errors.

## Performance
Use the Profiler and Frame Debugger. Measure lighting, shadows, transparent overdraw, post-processing, texture sizes, and material variants on the actual target.
