# 2D: Physics

## Guides
- [Queries and collision matrix](./Queries-and-Collision-Matrix.md)
- [Physics2D query reference](./Physics2D-Query-Reference.md)
- [Ground checks and LayerMasks](./Ground-Checks-and-Layer-Masks.md)
- [One-way platforms](./One-Way-Platforms.md)
- [Physics materials and friction](./Physics-Materials-and-Friction.md)
- [Projectile and damage sample](./Projectile-and-Damage.md)
- [Melee and area hitboxes](./Melee-and-Area-Hitboxes.md)

## Body types
- Dynamic: participates in force/gravity simulation and collision response.
- Kinematic: controlled by code, not driven by forces in the same way as Dynamic.
- Static: represents geometry that does not move under simulation.

## Callbacks and queries
Solid contact callbacks: OnCollisionEnter2D, OnCollisionStay2D, OnCollisionExit2D. Trigger callbacks: OnTriggerEnter2D, OnTriggerStay2D, OnTriggerExit2D. Queries include Raycast, CircleCast, BoxCast, and overlap methods.

Physics Material 2D controls friction and bounciness. The Physics 2D Layer Collision Matrix decides which layers interact. Use LayerMasks so ground checks and targeting ignore irrelevant objects.

## Troubleshooting
Confirm colliders are enabled and overlapping as expected, check trigger/Rigidbody configuration, verify layer settings, and use exact callback signatures. Use physics-step timing for physics-driven movement. [Physics2D API](https://docs.unity3d.com/ScriptReference/Physics2D.html)
