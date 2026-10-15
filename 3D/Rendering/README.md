# 3D: Rendering and Materials

## Guides
- [Lighting, materials, and URP](./Lighting-Materials-and-URP.md)
- [Camera and post-processing](./Post-Processing-and-Camera.md)
- [Particle systems and trails](./Particle-Systems-and-Trails.md)
- [Model import settings](../Modeling-and-Materials/Model-Import-Settings.md)
- [Material import troubleshooting](../Modeling-and-Materials/Material-Import-Troubleshooting.md)

## Materials and pipelines
A Material references a shader and shader properties. Built-in Render Pipeline, URP, and HDRP are distinct pipeline choices; shaders are not universally interchangeable. Pipeline migration may require converting/remapping materials.

## Camera and lighting
Inspect projection, field of view/orthographic size, clip planes, culling mask, lighting intensity, shadow settings, reflection probes, and post-processing. Pink/magenta usually indicates shader incompatibility or compilation failure.

## Effects and performance
Particle Systems create effects; Trail Renderers draw fading tracks. Limit particle count, unnecessary collision modules, transparent overdraw, shadows, and full-screen effects as appropriate. Profile target builds rather than guessing.
