# 3D Component Inspector Reference

Inspector labels and available fields vary with Unity version, package, and render pipeline. Treat this as a conceptual guide and use the Editor tooltips for the installed version.

## Rigidbody

| Property | Purpose / when to change it |
|---|---|
| Mass | Affects response to forces and collisions. |
| Drag / Linear Damping | Resists movement over time; label varies by version. |
| Angular Drag / Angular Damping | Resists rotation over time. |
| Use Gravity | Enables gravity for a Dynamic body. |
| Is Kinematic | Changes the body to code-driven physics behaviour. |
| Interpolate | Smooths rendered movement between physics updates. |
| Collision Detection | Discrete is cheaper; Continuous modes can help fast objects avoid tunnelling. |
| Constraints | Freezes position or rotation axes. |

## Collider
- **BoxCollider:** boxes, crates, walls, and simple rectangular shapes.
- **SphereCollider:** round objects and simple detection volumes.
- **CapsuleCollider:** characters and elongated rounded objects.
- **MeshCollider:** matches mesh geometry; more expensive and has restrictions for moving rigidbodies.
- **Is Trigger:** detects overlaps without ordinary solid blocking.
- **Material:** friction and bounciness.
- **Center / Size / Radius / Height:** define shape placement and dimensions.

## CharacterController
- **Slope Limit:** steepness the controller can climb.
- **Step Offset:** maximum step height the controller can step onto.
- **Skin Width:** tolerance around the capsule; too small can increase sticking/jitter.
- **Min Move Distance:** small moves below this may be ignored.
- **Center, Radius, Height:** define the capsule volume.

## MeshRenderer
- **Materials:** material slots used to render submeshes.
- **Shadow Casting / Receive Shadows:** shadow behaviour.
- **Light Probes / Reflection Probes:** indirect lighting data sources.
- **Motion Vectors / Rendering options:** pipeline-dependent rendering features.

## Camera
- **Projection:** Perspective or Orthographic.
- **Field of View:** perspective viewing angle.
- **Clipping Planes:** near and far visible distances.
- **Culling Mask:** layers rendered by the camera.
- **Viewport Rect:** portion of the screen occupied by the camera.

## Diagnostic order
Check active state, assigned references, layers, collider geometry, Rigidbody configuration, camera culling, material/shader compatibility, then Console errors.

Official references: [Rigidbody](https://docs.unity3d.com/ScriptReference/Rigidbody.html) · [Collider](https://docs.unity3d.com/ScriptReference/Collider.html) · [CharacterController](https://docs.unity3d.com/ScriptReference/CharacterController.html)
