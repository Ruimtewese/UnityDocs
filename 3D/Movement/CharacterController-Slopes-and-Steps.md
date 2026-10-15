# CharacterController: Slopes, Steps, and Skin Width

CharacterController movement depends strongly on its capsule size and its interaction with level geometry. If the character sticks, slides, or cannot climb small ledges, inspect both controller settings and scene colliders before adding code workarounds.

## Inspector properties
- Slope Limit: maximum slope angle the controller can climb.
- Step Offset: the height of steps it can automatically step onto; must be compatible with the controller height.
- Skin Width: collision tolerance around the capsule. Too small can increase sticking/jitter; too large can feel imprecise.
- Min Move Distance: very small moves below this may be ignored.
- Center / Height / Radius: the actual capsule shape. Ensure the feet align with the model and the capsule is not deeply embedded in the floor.

## Scene checks
1. Confirm stairs and ramps use continuous/simple collision geometry where possible.
2. Compare visual mesh and collider mesh; a decorative model may not be the walkable surface.
3. Test a low step, steep slope, edge, and narrow corridor separately.
4. Ensure movement code applies gravity and uses CharacterController.Move with displacement, not raw velocity.
5. Avoid adding a Rigidbody that independently drives the same character movement.

## Grounding and slope behavior
CharacterController.isGrounded describes grounding associated with the most recent Move. It is not a complete slope-normal query. For custom slope rules, perform a downward ray/sphere cast and inspect RaycastHit.normal. Define how to handle steep slopes: block, slide downward, or allow movement.

## Common mistakes
- Step Offset exceeds suitable character dimensions.
- Capsule center/height makes the feet float or intersects the floor.
- Slope limit permits surfaces the game should reject.
- A moving platform and controller each apply the same displacement.
- Rigidbody and CharacterController both try to own the same character.
