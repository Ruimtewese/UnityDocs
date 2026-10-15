# 3D: Movement

## Choose an approach
- **Rigidbody controller:** physics simulation drives movement and collision response.
- **CharacterController:** script calculates displacement and calls CharacterController.Move; gravity is normally applied by your own code.
- **Transform-driven movement:** suitable for non-physics objects or carefully controlled cases, but it does not provide normal physics collision response by itself.

## Camera-relative movement
Use the camera's forward/right directions, flatten them onto the ground plane, normalize as appropriate, then combine with the input vector. This makes controls follow the view direction without moving upward when the camera looks down.

## Rotation
Use quaternions for robust 3D orientation operations. Decide whether the character faces input direction, velocity, camera direction, or a target. Keep that convention consistent.

## Common mistakes
CharacterController.Move accepts a displacement and does not apply gravity automatically. Direct Transform motion can conflict with Rigidbody control. Clamp vertical movement and test slopes, steps, and moving platforms deliberately.

Reference: [CharacterController API](https://docs.unity3d.com/ScriptReference/CharacterController.html)
