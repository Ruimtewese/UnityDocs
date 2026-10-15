# 2D: Optimization

Measure before optimizing. Use the Unity Profiler on a representative build and device.

## Areas to inspect
- Texture memory, sprite atlas usage, compression, and asset dimensions.
- Transparent overdraw from large overlapping sprites and effects.
- Frequent instantiate/destroy patterns for projectiles, enemies, and particles.
- Physics query frequency, collider complexity, and layer masks.
- UI rebuilds, allocations, and unnecessary per-frame work.

## Practical approach
Record a baseline, identify the bottleneck, change one thing, then profile again. Object pooling and atlases can help in the right context, but neither is automatically faster for every project. Test builds rather than relying solely on Editor performance.

Reference: [Unity Profiler](https://docs.unity3d.com/Manual/Profiler.html)
