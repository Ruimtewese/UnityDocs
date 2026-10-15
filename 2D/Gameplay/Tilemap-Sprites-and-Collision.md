# 2D Tilemaps, Sprite Import, and Collision

## Sprite import
Set Texture Type to Sprite (2D and UI). Use Single for one sprite or Multiple for a sheet. Slice multiple sprites in Sprite Editor. Pixels Per Unit determines how many source pixels correspond to one world unit. Filtering and compression depend on art style and target; pixel art commonly uses Point filtering and careful camera/PPU alignment.

## Tilemap workflow
1. Create a 2D Tilemap, usually Rectangular.
2. Create a Tile Palette and drag sprites in to create Tile assets.
3. Separate decorative tiles from solid collision tiles when practical.
4. Add TilemapCollider2D to the collision Tilemap.
5. For large maps, consider CompositeCollider2D to merge adjacent tile boundaries. Configure the TilemapCollider2D composite operation and Rigidbody2D setup according to the installed Unity version.
6. Set the collision Tilemap's layer and verify the Physics 2D collision matrix.

## Collision best practices
- Use composite geometry where it reduces seams and collider overhead.
- Inspect collider outlines in Scene view.
- Check Tile asset Collider Type and TilemapRenderer sorting settings.
- Ensure player Rigidbody2D and collider match the movement controller.
- Keep collision and decorative layers separate if their behavior differs.

## Common problems
- Player falls through: missing/disabled collider, layer matrix, or incompatible body setup.
- Player catches on seams: try a composite collider and tune collider geometry.
- Wrong render order: configure Sorting Layer and Order in Layer.
- Blurry pixel art: check filter mode, PPU, camera scaling, and pixel-perfect setup.
