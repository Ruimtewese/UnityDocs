# 3D: Movement

## Guides
- [CharacterController guide](./CharacterController-Guide.md)
- [Jumping and ground checks](./Jumping-and-Ground-Checks.md)
- [Rigidbody movement](./Rigidbody-Movement.md)
- [Camera-relative Rigidbody movement](./Camera-Relative-Rigidbody-Movement.md)
- [Rigidbody forces](../Physics/Forces-and-Rigidbody-Movement.md)

## Choose an approach
- Rigidbody controller: physics simulation drives movement and collision response.
- CharacterController: script calculates displacement and calls Move; gravity is normally implemented in code.
- Transform-driven movement: suitable for non-physics objects or controlled cases, but does not itself provide ordinary physics collision response.

## Camera-relative movement
Flatten camera forward/right onto the ground plane, normalize as appropriate, then combine with input. This makes controls follow view direction without moving upward when the camera looks down.

## Rotation
Use quaternions for robust 3D orientation operations. Decide whether the character faces input direction, velocity, camera, or a target and keep that convention consistent.

## Common mistakes
CharacterController.Move accepts displacement and does not apply gravity automatically. Transform movement can conflict with Rigidbody control. Clamp vertical motion and test slopes, steps, and moving platforms.

References: [CharacterController API](https://docs.unity3d.com/ScriptReference/CharacterController.html) · [Rigidbody API](https://docs.unity3d.com/ScriptReference/Rigidbody.html)
