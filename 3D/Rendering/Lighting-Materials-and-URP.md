# 3D Rendering: Materials, Lighting, and URP

## Materials
A Material references a shader and surface properties. Common properties include Base Map/Color, Metallic, Smoothness, and Normal Map. The shader determines how these properties are interpreted.

Built-in Render Pipeline, URP, and HDRP shaders are not universally interchangeable. Choose a render pipeline intentionally; migration can require material conversion and validation.

## Lighting checklist
- Directional Light for sun-like illumination; Point Light for local bulbs; Spot Light for cones.
- Use shadows where they improve readability; shadow maps cost performance.
- Consider baked lighting for static environments and real-time lighting for dynamic needs.
- Reflection Probes improve environment reflections when supported by the pipeline/shader.
- Check color space, exposure, and post-processing volumes when a scene looks too dark or washed out.

## Pink/magenta materials
Pink commonly indicates a missing or incompatible shader. Check the active render pipeline, shader compatibility, pipeline asset assignments in Graphics/Quality settings, material conversion after pipeline changes, and custom shader compilation. Do not replace every material blindly.

## Performance
Use Profiler and Frame Debugger. Reduce unnecessary real-time lights, transparent overdraw, excess material variants where batching helps, and textures larger than the target requires. Measure instead of assuming an optimization always helps.
