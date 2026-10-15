# 2D: Tilemaps

## Main pieces
- **Grid:** defines cell layout and spacing.
- **Tilemap:** stores tile data for cells.
- **Tilemap Renderer:** draws the tilemap.
- **Tile Palette:** editor workflow for painting tiles.
- **Tilemap Collider 2D:** builds collision shapes from tiles.
- **Composite Collider 2D:** can combine adjacent outlines when configured with a suitable Rigidbody2D setup.
- **Rule Tiles:** can select sprites based on neighbours when the required package is installed.

## Layer design
Use separate tilemaps for ground, decoration, foreground, collision, hazards, and interactive tiles when their sorting or collision needs differ. Not every decorative tile needs a collider.

## Troubleshooting
Check the Tile Palette asset, active tilemap, sorting layer, collider enable state, and physics layer matrix. When using Composite Collider 2D, verify the Rigidbody2D and composite operation settings.

Reference: [Tilemap manual](https://docs.unity3d.com/Manual/Tilemap.html)
