# 2D: Cameras

## Orthographic camera
Orthographic Size is half the vertical view span in world units. The aspect ratio determines the horizontal span. Position the camera so the gameplay plane is visible and the camera's clipping planes include the sprites.

## Follow camera
A basic follow camera can use a target Transform and smoothing, but tune damping so it does not make controls feel delayed. For more complex framing, camera blends, confiners, or impulses, consider Cinemachine.

## Pixel-art checklist
- Keep PPU and asset sizing consistent.
- Use an intentional reference resolution.
- Use point filtering when crisp pixels are required.
- Consider pixel-perfect camera tools and avoid fractional camera motion that causes shimmer.
- Test several aspect ratios and target devices.

## Common mistakes
Orthographic Size is not the full vertical world height. Excessive smoothing creates camera lag. Camera bounds should be based on the playable area, not guessed from the screen.

Reference: [Camera API](https://docs.unity3d.com/ScriptReference/Camera.html)
