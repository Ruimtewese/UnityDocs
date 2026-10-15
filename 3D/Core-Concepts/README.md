# 3D: Core Concepts

For vector operations, coordinate conversion, quaternion rotation, and time-step concepts, see [Transforms and Essential Math](./Transforms-and-Math.md).

## Coordinate spaces
Unity commonly uses X/Z for ground-plane movement and Y for vertical height, though a project can choose other conventions. Transform.position is world space; localPosition is relative to the parent. Parent rotation and scale affect child world transforms.

## Geometry and rendering
- **Mesh:** vertices, triangles, normals, UVs, and related geometry data.
- **MeshFilter:** supplies mesh geometry to a MeshRenderer.
- **MeshRenderer:** renders a mesh with assigned materials.
- **Material:** stores shader and property values.
- **Prefab:** reusable GameObject setup with overrides on scene instances.
- **Perspective camera:** applies perspective; distant objects appear smaller.
- **Orthographic camera:** does not apply perspective scaling.

## Common mistakes
Mixing local and world coordinates causes placement bugs. Parent scaling can affect child dimensions and complicate physics. A MeshRenderer alone does not supply geometry.

Reference: [Transform API](https://docs.unity3d.com/ScriptReference/Transform.html) · [Mesh API](https://docs.unity3d.com/ScriptReference/Mesh.html)
