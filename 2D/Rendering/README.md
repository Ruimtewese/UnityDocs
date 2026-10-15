# 2D: Rendering

## Guides
- [Sorting layers and pixel art](./Sorting-Layers-and-Pixel-Art.md)
- [Parallax background](./Parallax-Background.md)

## Sprite rendering
SpriteRenderer draws a sprite with a selected material and color. Sorting Layer provides broad order; Order in Layer orders renderers within that layer. Z depth and transparency sorting can also matter depending on the camera/project settings.

## Import and pixels
Use Sprite (2D and UI) texture type. Choose Single for one sprite or Multiple for sheets. Keep Pixels Per Unit consistent. Point filtering is common for pixel art; Bilinear may suit smooth art. Test filter/compression settings at target resolution.

## Parallax
Move distant background layers by a fraction of camera movement. Use camera delta rather than repeatedly adding absolute camera position, and keep each layer's movement owned by one script.

## Common mistakes
Wrong sorting layer/order, mismatched PPU, inconsistent pivots, fractional camera movement in pixel art, and background Z/order that covers gameplay.
