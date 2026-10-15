# 3D Physics: Rigidbody, Colliders, Layers, and Triggers

## Rigidbody Inspector
- Mass affects momentum and force response; mass alone does not make objects fall faster under ordinary gravity.
- Linear Damping resists motion over time; Angular Damping resists rotation.
- Use Gravity applies scene gravity.
- Is Kinematic means physics forces do not drive motion; use supported Rigidbody APIs.
- Interpolate smooths rendered motion between physics updates.
- Collision Detection: Discrete is cheaper; Continuous modes can reduce tunneling for fast bodies at added cost.
- Constraints freeze unwanted axes or rotation.

Unity 6 uses Rigidbody.linearVelocity in current examples; older versions commonly use velocity. Check the installed API.

## Collider types
BoxCollider, SphereCollider, and CapsuleCollider are efficient primitives. MeshCollider fits complex static geometry; convex restrictions apply to dynamic Rigidbody interactions. Is Trigger sends trigger callbacks without solid collision response. Physic Material configures friction and bounciness. Prefer simple colliders over detailed render meshes.

## Layers
The Layer Collision Matrix is under Edit → Project Settings → Physics. Disable pairs that should never collide and use LayerMask for raycasts. A GameObject Layer is not a 2D Sorting Layer.

## Callback prerequisites
Collision and trigger callbacks require correctly configured colliders and Rigidbody relationships. The 2D and 3D physics APIs are separate: Collider2D/Rigidbody2D callbacks are not interchangeable with Collider/Rigidbody callbacks.

Common mistakes: setting Transform position every frame on a dynamic Rigidbody; fast projectile tunneling; collision disabled in the layer matrix; trigger setup missing a suitable Rigidbody; or using a non-convex MeshCollider in a dynamic setup that requires convex.
