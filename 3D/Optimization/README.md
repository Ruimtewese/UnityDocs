# 3D: Optimization

Profile a representative build before changing architecture. Use the Unity Profiler for CPU/GPU/memory data and the Frame Debugger for rendering analysis.

## Areas to investigate
- Mesh complexity, LOD Groups, and occlusion culling suitability.
- Draw calls, material variants, shader complexity, and real-time shadows.
- Texture dimensions, compression, mipmaps, and memory usage.
- Physics query frequency, collider complexity, and Rigidbody count.
- Per-frame allocations, unnecessary Update work, and repeated scene searches.
- Object creation/destruction churn and whether pooling improves measured performance.

## Workflow
1. Capture a baseline on target hardware.
2. Identify the actual bottleneck.
3. Change one relevant factor.
4. Re-profile and compare.
5. Test visual quality and gameplay behaviour after optimization.

Do not assume polygon count is the only cost. GPU fill rate, overdraw, shader cost, CPU scripts, physics, and memory can all be limiting factors.

Reference: [Unity Profiler](https://docs.unity3d.com/Manual/Profiler.html)
