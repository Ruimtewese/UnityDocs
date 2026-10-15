# 3D: Optimization and Project Quality

## Guides
- [Build and performance checklist](./Build-and-Performance-Checklist.md)
- [Memory and asset loading](./Memory-and-Asset-Loading.md)
- [Version control for Unity](./Version-Control-for-Unity.md)
- [Unity Test Framework quickstart](./Unity-Test-Framework-Quickstart.md)

## Profile-driven workflow
Establish a baseline on target hardware; measure CPU, GPU, memory, garbage collection, physics, rendering, and AI; change one thing; measure again. Do not optimize based only on intuition or Editor performance.

## Typical costs
- Frequent Instantiate/Destroy and per-frame allocations.
- Large textures/meshes/audio kept in memory.
- Complex colliders and excessive physics bodies.
- Real-time lights, shadows, transparency, and post-processing.
- Unnecessary scene-wide searches and over-frequent AI path updates.

## Build quality
Commit ProjectSettings and package manifest/lock files. Keep .meta files in version control. Test missing/corrupt save data, scene transitions, platform input, and target-device performance.

## Official references
[Unity Profiler](https://docs.unity3d.com/Manual/Profiler.html) · [Unity Test Framework](https://docs.unity3d.com/6000.0/Manual/com.unity.test-framework.html)
