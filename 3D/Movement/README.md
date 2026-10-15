# 3D: Movement

## Guides
- [CharacterController guide](./CharacterController-Guide.md)
- [CharacterController slopes and steps](./CharacterController-Slopes-and-Steps.md)
- [Jumping and ground checks](./Jumping-and-Ground-Checks.md)
- [Rigidbody movement](./Rigidbody-Movement.md)
- [Camera-relative Rigidbody movement](./Camera-Relative-Rigidbody-Movement.md)
- [Rigidbody jump and ground check](./Rigidbody-Jump-and-Ground-Check.md)
- [Rigidbody forces](../Physics/Forces-and-Rigidbody-Movement.md)

## Choose an approach
- Rigidbody controller: physics drives movement and collision response.
- CharacterController: script calculates displacement and calls Move; gravity is normally implemented in code.
- Transform-driven movement: suitable for non-physics objects or controlled cases, but does not provide ordinary physics collision response.

## Camera-relative movement
Flatten camera forward/right onto the ground plane, normalize as appropriate, then combine with input. This makes controls follow view direction without moving upward when the camera looks down.

## Rotation and troubleshooting
Use quaternions for 3D orientation. Decide whether the character faces input direction, velocity, camera, or target. CharacterController.Move accepts displacement and does not apply gravity. Transform movement can conflict with Rigidbody control. Test slopes, steps, moving platforms, and edges.
