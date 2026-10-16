# Pixel-art import settings

Consistent import settings are essential for crisp tile-based and sprite-based games.

## Recommended starting point

For each pixel-art texture:

- **Texture Type:** Sprite (2D and UI)
- **Sprite Mode:** Single or Multiple for a sheet
- **Pixels Per Unit:** choose a consistent value across tiles and characters
- **Filter Mode:** Point (no filter)
- **Compression:** None for strict pixel fidelity, or test platform compression visually
- **Generate Mip Maps:** usually off for flat 2D pixel art
- **Mesh Type:** Tight or Full Rect depending on slicing and batching needs

Settings may vary by art style and target platform; verify the result in a built player as well as the Editor.

## Pixels Per Unit example

If a tile is 16 pixels wide and PPU is 16, it occupies about one world unit. A 32-pixel tile at PPU 32 also occupies one world unit. The player sprite, tilemaps, and camera should use a consistent scale.

## Sprite sheet slicing

1. Open the texture's Sprite Editor.
2. Choose Slice and select Grid by Cell Size or Cell Count.
3. Enter the actual cell dimensions.
4. Inspect each slice for padding and alignment.
5. Apply, then verify the sprites in the Project window.

## Common problems

- Blurry sprites: check filtering, compression, PPU, and camera scaling.
- Lines between tiles: inspect sprite padding, atlas settings, and subpixel camera positions.
- Different sprite sizes: standardize PPU or intentionally scale individual assets.
- Camera shimmer: use a pixel-perfect camera workflow if exact pixel alignment is a requirement.
