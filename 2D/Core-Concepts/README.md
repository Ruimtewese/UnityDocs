# 2D: Core Concepts

Guides for coordinate spaces, sprite sizing, lifecycle, data, C#, timing, and object reuse.

## Guides
- [Coordinate spaces and Pixels Per Unit](./Coordinate-Spaces-and-PPU.md)
- [C# essentials for Unity](./CSharp-For-Unity.md)
- [Script lifecycle and architecture](./Script-Lifecycle-and-Architecture.md)
- [Prefabs, ScriptableObjects, and data](./Prefabs-ScriptableObjects-and-Data.md)
- [Time, coroutines, and Invoke](./Time-Coroutines-and-Invoke.md)

## Key rules
X/Y commonly describe the gameplay plane, while Z can affect depth/sorting. Orthographic Size is half the vertical view height. Screen pixels and world units are different spaces. PPU is the number of source pixels represented by one world unit. Sorting layers control draw order, not physics collisions.

Keep runtime state separate from shared authored data. Use one clear owner for each value such as health, movement, pause state, or audio volume.

## Common mistakes
Inconsistent PPU makes assets look mismatched; fractional camera positions can blur pixel art; local/world coordinates get confused; Transform and Rigidbody scripts fight; dictionaries and ordinary properties do not automatically serialize using Unity's default serializer.

References: [Transform API](https://docs.unity3d.com/ScriptReference/Transform.html) · [Unity Manual](https://docs.unity3d.com/Manual/index.html)
