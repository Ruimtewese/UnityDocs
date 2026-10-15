# 3D: Cameras

## Guides
- [First- and third-person camera foundations](./First-and-Third-Person-Camera.md)
- [Third-person camera collision and smoothing](./Camera-Collision-and-Smoothing.md)

## Key camera settings
Projection selects Perspective or Orthographic. Perspective Field of View controls the viewing angle; near/far clipping planes define visible depth. Culling Mask selects rendered layers. Check transform axes and local versus world positions when setting a camera offset.

## First person
Separate character yaw from camera pitch. Lock the cursor only during gameplay and unlock it for menus/pause.

## Third person
Use a target pivot and a desired offset behind the player. Add wall-collision handling, avoid colliding with the player itself, and integrate camera smoothing into one system. Cinemachine may be preferable for complex follow rigs and confiners.

## Common problems
- Camera clips through walls or the player.
- Mouse look rotates movement axes incorrectly.
- Cursor remains locked while settings are open.
- Camera clipping planes or culling mask hide objects.
