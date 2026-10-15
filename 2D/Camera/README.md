# 2D: Cameras

## Guides
- [Camera follow and pixel-perfect setup](./Camera-Follow-and-Pixel-Perfect.md)
- [Camera bounds and level confinement](./Camera-Bounds.md)
- [Screen shake recipe](../Recipes/Screen-Shake.md)

## Orthographic projection
Orthographic Size is half of the visible vertical world span. Visible width depends on aspect ratio. Set a deliberate camera Z position and ensure the gameplay plane is in front of the camera.

## Follow and confinement
Use LateUpdate for many simple follow cameras, or a camera package such as Cinemachine for richer rigs. Bounds must account for the camera's half-width and half-height. Integrate smoothing and clamping so one does not undo the other.

## Pixel art
Keep PPU/reference resolution consistent and test actual target resolution. Fractional camera positions can create shimmer or uneven pixel movement.
