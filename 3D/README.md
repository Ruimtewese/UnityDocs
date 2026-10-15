# 3D Development

A practical reference for 3D worlds, first- and third-person games, physics, materials, lighting, cameras, and rendering.

## Topic index
- [Core Concepts](./Core-Concepts/README.md)
- [Components](./Components/README.md)
- [Movement](./Movement/README.md)
- [Physics](./Physics/README.md)
- [Input](./Input/README.md)
- [Animation](./Animation/README.md)
- [Cameras](./Cameras/README.md)
- [Modeling and Materials](./Modeling-and-Materials/README.md)
- [UI](./UI/README.md)
- [Audio](./Audio/README.md)
- [Lighting and Rendering](./Lighting-and-Rendering/README.md)
- [Optimization](./Optimization/README.md)
- [Recipes](./Recipes/README.md)

## Recommended learning order
1. Learn Transform spaces, meshes, materials, prefabs, and perspective.
2. Understand Rigidbody, Collider, MeshRenderer, and CharacterController.
3. Build movement, input, collision queries, and camera control.
4. Add animation, interaction, UI, and audio.
5. Configure materials, lighting, and the chosen render pipeline.
6. Profile builds and tune performance.

## 3D-specific essentials
- Unity commonly uses X/Z for ground-plane movement and Y for vertical height, but this is a convention, not a universal requirement.
- Transform.position is world space; localPosition is relative to the parent.
- A MeshFilter supplies mesh geometry and a MeshRenderer draws it.
- Rigidbody-driven movement and CharacterController movement are different approaches; avoid mixing them casually.
- Built-in, URP, and HDRP workflows are not fully interchangeable.

Official references: [Unity Manual](https://docs.unity.com/en-us) · [Scripting API](https://docs.unity3d.com/ScriptReference/)
