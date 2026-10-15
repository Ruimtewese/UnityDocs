# 3D: Optimization and Project Quality

## Guides
- [Build and performance checklist](./Build-and-Performance-Checklist.md)
- [Memory and asset loading](./Memory-and-Asset-Loading.md)
- [Addressables versus Resources](./Addressables-and-Resources.md)
- [Version control for Unity](./Version-Control-for-Unity.md)
- [Unity Test Framework quickstart](./Unity-Test-Framework-Quickstart.md)

## Profile-driven workflow
Establish a baseline on target hardware; measure CPU, GPU, memory, garbage collection, physics, rendering, and AI; change one thing; measure again. Do not optimize based only on intuition or Editor performance.

## Typical costs
Frequent Instantiate/Destroy, per-frame allocations, large assets kept in memory, complex colliders, excessive real-time lights/shadows, transparency, post-processing, scene-wide searches, and over-frequent AI path updates.

## Build quality
Commit ProjectSettings and package manifest/lock files. Keep .meta files in version control. Test missing/corrupt save data, scene transitions, platform input, and target-device performance.

Official references: [Unity Profiler](https://docs.unity3d.com/Manual/Profiler.html) · [Runtime asset management](https://docs.unity3d.com/6000.0/Documentation/Manual/assets-managing-introduction.html) · [Test Framework](https://docs.unity3d.com/6000.0/Manual/com.unity.test-framework.html)
