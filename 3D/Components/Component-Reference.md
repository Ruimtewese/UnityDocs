# Common 3D Components: What to Inspect

This is a practical overview, not a substitute for the version-specific Manual or API.

## Transform
Position, rotation, scale, and parent/child hierarchy. Confirm world versus local space. Negative scale and non-uniform parent scale can produce confusing orientation and physics behavior.

## Rigidbody
Simulates motion and physical response. Key settings: Mass, Linear/Angular Damping, Use Gravity, Is Kinematic, Interpolate, Collision Detection, Constraints. Avoid writing Transform position every frame on a dynamic body. Unity 6 exposes linearVelocity; older APIs may use velocity.

## Collider types
- BoxCollider: boxes and simple hitboxes.
- SphereCollider: round objects and triggers.
- CapsuleCollider: characters and elongated objects.
- MeshCollider: geometry-shaped collision; dynamic use has convex limitations.
- Is Trigger: overlap events instead of solid contact response.

Tune collider center/size/radius/height independently of visible mesh where practical.

## CharacterController
A capsule-based character mover using Move or SimpleMove. Move does not automatically apply gravity; gameplay code must handle vertical velocity. Inspect Height, Radius, Center, Slope Limit, Step Offset, Skin Width, and Min Move Distance.

## Camera
Projection (Perspective/Orthographic), Field of View or Orthographic Size, Clipping Planes, Culling Mask, Depth, and pipeline properties. A small far clip or wrong culling mask can make objects disappear.

## MeshRenderer / SkinnedMeshRenderer
Materials, shadow casting/receiving, probe settings, and enabled state. SkinnedMeshRenderer also requires a valid rig and skin weights.

## Animator
Controller, Avatar, Apply Root Motion, Update Mode, Culling Mode. Parameter names/types and transition conditions must match the controller.

## AudioSource
Clip, Volume, Pitch, Loop, Play On Awake, Spatial Blend, Min/Max Distance, Output Mixer Group. Usually keep one active AudioListener on the player camera.

## Canvas / EventSystem
Canvas render mode and sorting; Canvas Scaler and anchors; EventSystem plus an input module matching the project input backend. For uGUI clicks, inspect Graphic Raycaster, Raycast Target, and CanvasGroup.

## Troubleshooting
If a component appears ineffective, verify it is enabled, its GameObject is active, references are assigned, layers/masks permit interaction, the correct physics system is used, and the Scene/Game views are not hiding the result.
