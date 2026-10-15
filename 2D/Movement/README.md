# 2D: Movement

Choose a movement authority and keep it consistent. Separate input collection from movement so controls can later be replaced by AI, replay, or touch input.

## Guides
- [Platformer Controller](./Platformer-Controller.md)
- [Top-Down Movement](./Top-Down-Movement.md)
- [Moving Platforms](./Moving-Platforms.md)
- [Ground checks and LayerMasks](../Physics/Ground-Checks-and-Layer-Masks.md)

## Platformer checklist
Horizontal acceleration/deceleration, a small ground overlap/cast, jump buffering/coyote time where desired, variable jump height, terminal fall speed, and moving-platform behavior. Keep sprite facing separate from physics velocity when appropriate.

## Top-down checklist
Read Vector2 input, normalize if diagonal movement should not be faster, and choose whether the character rotates or flips. Use a consistent body/physics approach.

## Physics timing
For Dynamic Rigidbody2D movement, use Rigidbody2D APIs with physics-step timing. For kinematic movement, understand MovePosition behavior and collision limits. Do not mix arbitrary Transform movement and Rigidbody control without a deliberate design.

## Debugging
Draw the ground probe, verify LayerMasks and collider geometry, and log input/grounded state. A single-frame collision flag can be a fragile substitute for a ground check.

Reference: [Rigidbody2D API](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html)
