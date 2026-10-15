# 2D: Core Concepts

Understand the 2D coordinate plane, sprites, pixels per unit, sorting, orthographic projection, and asset import.

## Coordinates and camera
- X/Y usually describe gameplay movement; Z can affect depth and sorting configuration.
- Orthographic Size is half the vertical view span in world units; full vertical span is twice that value.
- Screen pixels and world units are different spaces. Convert screen positions through the active camera when necessary.

## Sprites and import settings
- **Texture Type:** Sprite (2D and UI) for sprite assets.
- **Sprite Mode:** Single for one sprite, Multiple for a sheet that will be sliced.
- **Pixels Per Unit:** number of source pixels represented by one world unit.
- **Filter Mode:** Point for crisp pixel art; Bilinear/Trilinear for smoothing appropriate textures.
- **Compression:** reduces memory at possible visual cost; check the target platform.
- **Pivot:** controls the sprite's local origin and can affect rotation and placement.

## Draw order
Sorting Layer establishes a broad draw group; Order in Layer orders renderers within it. Transparency sorting settings and Z can also matter depending on project configuration.

## Common mistakes
- Inconsistent PPU makes assets appear at mismatched sizes.
- Fractional camera positions can make pixel art shimmer.
- Z position is not a universal replacement for sorting layers.
- Sprite import changes may need Apply or reimporting to take effect.

See also: [SpriteRenderer API](https://docs.unity3d.com/ScriptReference/SpriteRenderer.html) · [Camera API](https://docs.unity3d.com/ScriptReference/Camera.html)
