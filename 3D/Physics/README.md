# 3D: Physics

## Rigidbody and Collider
Dynamic Rigidbody objects respond to forces and collision response. Kinematic bodies are controlled by code. Static colliders describe non-moving level geometry. Use appropriate shapes and avoid unnecessarily complex MeshColliders.

## Useful callbacks and queries
- Collisions: OnCollisionEnter, OnCollisionStay, OnCollisionExit.
- Triggers: OnTriggerEnter, OnTriggerStay, OnTriggerExit.
- Queries: Physics.Raycast, SphereCast, CapsuleCast, BoxCast, and overlap methods.
- Physic Material controls friction and bounciness.
- Layer Collision Matrix and LayerMasks filter interactions and queries.

## Reliable query checklist
Choose a clear origin, direction, maximum distance, and LayerMask. Exclude the player's own layer when necessary. Visualize rays in the Scene view during debugging.

## Troubleshooting
If callbacks fail, inspect collider/Rigidbody configuration, trigger state, layer matrix, active objects, and exact callback signatures. Use physics-step timing for physics-driven changes. Fast objects may need appropriate collision detection settings.

Reference: [Physics API](https://docs.unity3d.com/ScriptReference/Physics.html)
