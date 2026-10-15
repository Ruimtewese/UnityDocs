# Unity Memory and Asset Loading Basics

## Understand what costs memory
Textures, meshes, audio clips, animation data, instantiated GameObjects, and CPU-side copies can all consume memory. The exact footprint depends on platform, compression, import settings, and runtime use.

## Practical rules
- Use Read/Write on imported meshes only when runtime CPU access is required; it can keep an additional copy in memory.
- Choose texture sizes and compression for actual on-screen use and target platform.
- Select audio import/load settings based on clip duration, latency, CPU, and memory requirements.
- Unload or release assets when their lifetime truly ends; don't unload assets still referenced by active objects.
- Pool high-churn objects if profiling justifies it.
- Avoid keeping references to unused large assets in long-lived managers.
- Profile memory in a target build and take snapshots before guessing.

## Asset loading choices
- Direct serialized references are simple and appropriate for small projects.
- Resources provides runtime path-based loading but requires careful memory/lifetime management and can package more content than intended.
- Addressables is designed for asynchronous loading and explicit asset dependency management, especially for larger projects or downloadable content; it adds package/configuration complexity.
- AssetBundles are a lower-level content distribution mechanism and require explicit versioning/dependency management.

## Common mistakes
- Treating texture file size on disk as its in-memory size.
- Loading many large assets at scene start without measuring.
- Calling unload while a live object still needs the asset.
- Using Resources as an unstructured global asset database.
- Introducing Addressables without managing handles and release lifetimes.

Official docs: https://docs.unity3d.com/Manual/LoadingResourcesatRuntime.html
