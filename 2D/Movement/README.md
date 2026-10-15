# 2D: Movement

Movement patterns differ between platformers and top-down games. Separate input collection from movement so controls can later be replaced by AI or replay input.

## Platformer checklist
- Horizontal acceleration and deceleration.
- Ground detection using a small overlap or cast and a LayerMask.
- Jump input, jump buffering, and coyote time if desired.
- Variable jump height, terminal fall speed, and moving-platform behaviour.
- Separate facing direction from physics velocity when needed.

## Top-down checklist
- Read a Vector2 movement input.
- Normalize when diagonal movement should not be faster.
- Apply movement through the chosen controller consistently.
- Decide whether the character rotates toward movement or flips a sprite.

## Physics timing
For Dynamic Rigidbody2D movement, use Rigidbody2D velocity/forces or other supported Rigidbody2D APIs with physics-step timing. For kinematic movement, understand MovePosition and its collision limitations. Do not mix arbitrary Transform movement and Rigidbody control without a deliberate reason.

## Debugging
Draw or inspect the ground probe, verify its LayerMask, check the collider shape, and log the input/grounded state. A single-frame collision flag is often a fragile substitute for a ground check.

Reference: [Rigidbody2D API](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html)
