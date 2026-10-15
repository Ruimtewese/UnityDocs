# Unity Asset Loading: Direct References, Resources, and Addressables

Unity has several runtime asset workflows. The right choice depends on project size, memory budget, remote content needs, and whether assets must be downloaded or updated separately.

Official Unity 6 overview: https://docs.unity3d.com/6000.0/Documentation/Manual/assets-managing-introduction.html

## Choose the simplest system that fits
- **Direct Inspector references:** best default for many small games with fixed content. Assign prefabs/assets through serialized fields. Unity collects referenced assets into builds.
- **Resources:** convenient for prototypes or a small set of assets loaded by string at runtime. Everything under Resources is included in the build, and loaded assets need deliberate memory management. Avoid using it as a giant unstructured asset database.
- **Addressables:** useful for asynchronous loading, dependency management, remote content, and incremental content updates. Adds package setup, build catalog, address/group configuration, and handle lifecycle obligations.
- **AssetBundles:** a lower-level packaging/download approach. You must manage dependencies, versions, memory, and unloading more manually.

## Addressables workflow
1. Install Addressables using Package Manager.
2. Open Window → Asset Management → Addressables and create the settings when prompted.
3. Mark the intended assets Addressable and assign stable addresses/labels.
4. Build Addressables content for the appropriate target.
5. Load through Addressables APIs and check the async operation status before using a result.
6. Release handles/instances when their lifetime ends. Do not release an asset still used by an active object.
7. For remote content, configure and test hosting URLs, catalogs, caching, and versioning on the target platform.

## Lifetime rules
Loading an asset and instantiating a prefab are separate operations. An instantiated object can depend on the loaded prefab asset. Addressables operations return handles that need an intentional lifetime; releasing the wrong handle or too early can break references. Use the exact workflow documented by your package version.

## When not to use Addressables
A small game that ships all content up front may be simpler with direct references. Add Addressables when you need its loading/update/dependency features, not just because it exists.

## Common mistakes
- Addressables marked but content not built for the intended platform.
- Addressable address/label mismatch.
- A load fails but code still dereferences the result.
- Handle is never released, or is released while still needed.
- Resources folder silently increases build size.
- Testing only in Editor, not with a built player and configured remote URLs.
