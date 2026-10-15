# 2D: Components

## Common components
- **SpriteRenderer:** draws a sprite. Important fields include Sprite, Color, Flip X/Y, Draw Mode, Mask Interaction, Sorting Layer, and Order in Layer.
- **Rigidbody2D:** controls 2D simulation. Review Body Type, Simulated, Mass, Gravity Scale, Linear/Angular Damping (names vary by version), Collision Detection, Interpolate, and Constraints.
- **Collider2D:** defines a collision shape. Common types: Box, Circle, Capsule, Polygon, Edge, Composite.
- **Animator:** evaluates an Animator Controller and its parameters to select visual states.
- **SortingGroup:** sorts a group of renderers together, useful for multi-sprite characters.

## Choosing components
Use the simplest collider that supports the gameplay. Add Rigidbody2D when an object needs physics simulation or relevant collision/trigger interactions. Static level geometry generally does not need to be moved by script.

## Inspector checklist
1. Are references assigned?
2. Is the Rigidbody2D Body Type appropriate?
3. Are colliders enabled and sized correctly?
4. Is Is Trigger set as intended?
5. Do the involved layers interact in the Physics 2D Layer Collision Matrix?
6. Are sorting layer and order correct?

## Common mistakes
A collider alone does not make an object move. Directly changing Transform on a physics body can produce unreliable contacts. A trigger callback can fail due to missing Rigidbody2D setup, disabled objects, or layer filtering.

Official reference: [2D physics components](https://docs.unity3d.com/Manual/Physics2DReference.html)
