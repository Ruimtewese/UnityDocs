# 2D Physics Materials, Friction, and Bounciness

Physics Material 2D controls friction and bounciness on Collider2D components. It is different from a rendering Material. Create it through Assets → Create → 2D → Physics Material 2D, then assign it to a Collider2D's Material field.

## Friction
High friction resists sliding during contact. Low friction lets objects slide more easily. If the player stops abruptly when input ends, friction may contribute, but movement code also matters; a controller that writes velocity directly can override friction's expected effect.

## Bounciness
Bounciness controls how much collision velocity is retained on impact. A bounce may feel different with different masses, velocities, contact geometry, and combine settings. For predictable gameplay bounce pads, consider explicitly setting the player's vertical velocity when activated instead of relying entirely on material bounce.

## Combine behavior
When two colliders touch, Unity combines material values based on the configured friction/bounce combine modes. Labels and available options may vary by Unity version. Inspect both colliders when results seem unexpected.

## Troubleshooting
- Player slides: inspect material friction, Rigidbody2D damping, and movement code.
- Object barely bounces: inspect bounciness, combine modes, mass, and incoming speed.
- Collisions feel different on tile edges: inspect collider geometry and consider a CompositeCollider2D.
- The material changes surface appearance only: a rendering Material was assigned instead of Physics Material 2D.

Official API: https://docs.unity3d.com/ScriptReference/PhysicsMaterial2D.html
