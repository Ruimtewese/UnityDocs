# 3D: Physics

## Guides
- [Collision, layers, and Rigidbody](./Collision-Layers-and-Rigidbody.md)
- [Raycast interaction](./Raycast-Interaction.md)
- [Raycast/SphereCast/overlap reference](./Raycast-Spherecast-Overlap-Reference.md)
- [Rigidbody forces and movement](./Forces-and-Rigidbody-Movement.md)
- [Joints and constraints](./Joints-and-Constraints.md)
- [In-game physics query examples](./Projectile-and-Hitscan-Weapons.md)

## Core concepts
Dynamic Rigidbody objects respond to forces and collisions. Kinematic bodies are controlled by code. Static colliders describe non-moving level geometry. Use appropriate shapes and avoid unnecessarily complex MeshColliders.

Collisions use OnCollisionEnter/Stay/Exit; triggers use OnTriggerEnter/Stay/Exit. Queries include Raycast, SphereCast, CapsuleCast, BoxCast, and overlap methods. Physic Material controls friction and bounciness.

## Reliable query checklist
Choose origin, direction, maximum distance, LayerMask, and trigger behavior. Exclude the player's own layer when needed. Visualize rays/casts in the Scene view.

## Troubleshooting
Inspect collider/Rigidbody configuration, trigger state, layer matrix, active objects, and callback signatures. Fast objects may need appropriate collision detection. [Physics API](https://docs.unity3d.com/ScriptReference/Physics.html)
