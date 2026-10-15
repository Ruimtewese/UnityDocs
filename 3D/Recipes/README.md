# 3D: Recipes

Every recipe should include required GameObjects, components, input actions, layers, serialized references, complete code, setup instructions, expected result, and troubleshooting.

## Recommended recipes
1. Camera-relative third-person movement.
2. First-person camera look with input sensitivity.
3. Ground detection using a cast.
4. Raycast-based object interaction.
5. Projectile or object launch with physics.
6. Health and damage state.
7. Pickup and inventory counter.
8. Door, switch, and interactable interface.
9. Checkpoint and respawn.
10. Third-person camera obstruction handling.
11. World-space health bar.
12. Object pooling with measured benefit.

Use explicit LayerMasks and maximum distances for physics queries. Exclude the player's own collider when needed. Keep input, gameplay state, physics, audio, and UI responsibilities separate enough to test.
