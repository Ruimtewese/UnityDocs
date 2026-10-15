# 2D: Movement

Choose a movement authority and keep it consistent. Separate input collection from movement so controls can later be replaced by AI, replay, or touch input.

## Guides
- [Platformer controller](./Platformer-Controller.md)
- [Jump buffer and coyote time](./Jump-Buffer-and-Coyote-Time.md)
- [Top-down movement](./Top-Down-Movement.md)
- [Moving platforms](./Moving-Platforms.md)
- [Ground checks and LayerMasks](../Physics/Ground-Checks-and-Layer-Masks.md)

## Platformer checklist
Use horizontal acceleration/deceleration as desired, a small ground overlap/cast, jump buffering/coyote time, variable jump height, terminal fall speed, and deliberate moving-platform behavior. Keep sprite facing separate from physics velocity when appropriate.

## Top-down checklist
Read Vector2 input, normalize if diagonal movement should not be faster, and choose whether the character rotates or flips. Use a consistent physics approach.

## Physics timing
For Dynamic Rigidbody2D movement, use Rigidbody2D APIs with physics-step timing. For kinematic movement, understand MovePosition behavior and collision limitations. Avoid arbitrary Transform movement competing with Rigidbody control.

## Debugging
Draw the ground probe, verify LayerMasks and collider geometry, and log input/grounded state. Reference: [Rigidbody2D API](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html)
