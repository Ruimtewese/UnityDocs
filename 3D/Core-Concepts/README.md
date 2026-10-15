# 3D: Core Concepts

## Guides
- [Transforms and essential math](./Transforms-and-Math.md)
- [Common Unity APIs](./Common-Unity-APIs.md)
- [Vector math reference](./Vector-Math-Reference.md)
- [Math and Quaternion recipes](./Math-and-Quaternion-Recipes.md)
- [Coroutines, events, and async](./Coroutines-Events-and-Async.md)
- [C# events and interfaces](./Events-and-Interfaces.md)
- [Game architecture and scene flow](../Gameplay/Game-Architecture-and-Scene-Flow.md)

## Coordinate spaces
Unity commonly uses X/Z for ground-plane movement and Y for vertical height, though projects may choose another convention. Transform.position is world space; localPosition is relative to the parent. Parent rotation/scale affect child world transforms.

## Geometry and rendering terms
Mesh data includes vertices, triangles, normals, and UVs. MeshFilter supplies geometry to a MeshRenderer. Material references a shader and property values. Perspective cameras apply perspective; orthographic cameras do not scale apparent size with depth.

## Common mistakes
Mixing local/world coordinates, parent-scale side effects, using Lerp for constant speed, calling LookRotation with a zero direction, and assuming ordinary properties serialize automatically.

References: [Transform API](https://docs.unity3d.com/ScriptReference/Transform.html) · [Mesh API](https://docs.unity3d.com/ScriptReference/Mesh.html)
