# 3D: Components

## Common components
- **Transform:** world/local position, rotation, and scale.
- **MeshFilter + MeshRenderer:** static mesh geometry and rendering.
- **SkinnedMeshRenderer:** renders deforming meshes driven by a skeleton.
- **Rigidbody:** physics simulation, mass, drag/damping, gravity, kinematic state, interpolation, collision detection, and constraints.
- **Collider:** Box, Sphere, Capsule, Mesh, and Terrain colliders.
- **CharacterController:** collision-constrained movement driven by script rather than standard Dynamic Rigidbody simulation.
- **Animator:** evaluates animation states and parameters.

## Selection guidance
Use simple colliders where possible. A CharacterController and a Dynamic Rigidbody are different movement architectures. Avoid putting a non-convex MeshCollider on a moving Rigidbody unless the documented setup supports it.

## Inspector troubleshooting
Check references, collider dimensions, Rigidbody settings, enabled state, layer matrix, material assignments, and camera culling. Avoid unnecessary non-uniform scaling on physics hierarchies.

Reference: [Component API](https://docs.unity3d.com/ScriptReference/Component.html)
