# 2D Component Inspector Reference

Property names can change between Unity releases. This guide explains the purpose of common settings; use the installed Editor's tooltip and version-specific documentation for exact labels.

## SpriteRenderer

| Property | Purpose / when to change it |
|---|---|
| Sprite | Image asset drawn by this renderer. |
| Color | Tints the sprite; alpha can make it transparent. |
| Flip X / Flip Y | Mirrors the rendered sprite without rotating the GameObject. |
| Draw Mode | Simple, Sliced, or Tiled behaviour where supported. |
| Sorting Layer | Broad draw-order category. |
| Order in Layer | Relative draw order inside the sorting layer. |
| Mask Interaction | Controls interaction with Sprite Masks. |
| Material | Shader/material used to render the sprite. |

## Rigidbody2D

| Property | Purpose / caution |
|---|---|
| Body Type | Dynamic, Kinematic, or Static simulation behaviour. |
| Simulated | Whether the body participates in 2D physics. |
| Mass | Affects force response; changing mass does not necessarily change gravity acceleration. |
| Gravity Scale | Multiplies gravity for this body. |
| Linear / Angular Damping | Reduces linear/angular motion over time; labels vary by Unity version. |
| Collision Detection | Discrete is cheaper; Continuous may help fast bodies avoid tunnelling. |
| Interpolate | Smooths rendered movement between physics updates. |
| Constraints | Freeze position or rotation axes when appropriate. |

## Collider2D

| Property | Purpose / caution |
|---|---|
| Is Trigger | Reports overlap events instead of ordinary solid collision response. |
| Material | Physics Material 2D for friction/bounciness. |
| Offset | Moves the collider relative to its object. |
| Size / Radius / Points | Defines collision geometry, depending on shape. |
| Used by Composite | Lets compatible colliders contribute to a Composite Collider 2D. |

## Animator

- **Controller:** state machine asset to evaluate.
- **Avatar:** rig mapping where applicable.
- **Apply Root Motion:** applies animation-authored displacement; coordinate this with scripted movement.
- **Update Mode:** controls evaluation timing.
- **Culling Mode:** determines whether animation is evaluated when not visible.

## Diagnostic order
1. Confirm component enabled and GameObject active.
2. Confirm all serialized references are assigned.
3. Confirm layer and sorting configuration.
4. Confirm physics body type and collider dimensions.
5. Check Console errors and package/version mismatch.
6. Reproduce in a minimal test scene before changing many settings.

Official references: [SpriteRenderer](https://docs.unity3d.com/ScriptReference/SpriteRenderer.html) · [Rigidbody2D](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html) · [Collider2D](https://docs.unity3d.com/ScriptReference/Collider2D.html)
