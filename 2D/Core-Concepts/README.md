# 2D: Core Concepts

Guides for coordinate spaces, sprite sizing, lifecycle, data, C#, timing, tags, and localization.

## Guides
- [Coordinate spaces and Pixels Per Unit](./Coordinate-Spaces-and-PPU.md)
- [C# essentials for Unity](./CSharp-For-Unity.md)
- [Script lifecycle and architecture](./Script-Lifecycle-and-Architecture.md)
- [Prefabs, ScriptableObjects, and data](./Prefabs-ScriptableObjects-and-Data.md)
- [Time, coroutines, and Invoke](./Time-Coroutines-and-Invoke.md)
- [Tags, layers, sorting layers, and LayerMasks](./Tags-Layers-and-Sorting.md)
- [Localization and accessibility](./Localization-Setup.md)

## Key rules
X/Y commonly describe a 2D gameplay plane while Z can affect depth/sorting. Orthographic Size is half the vertical view height. Screen pixels and world units differ. PPU is the number of source pixels represented by one world unit. Sorting Layers control rendering order, not physics collisions.

Keep runtime state separate from authored data. Use one clear owner for health, movement, pause state, and other shared values.

References: [Transform API](https://docs.unity3d.com/ScriptReference/Transform.html) · [Unity Manual](https://docs.unity3d.com/Manual/index.html)
