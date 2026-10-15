# Tilemap Collision Workflow

## Recommended hierarchy
```text
Grid
├── GroundTilemap
├── DecorationTilemap
├── CollisionTilemap
└── ForegroundTilemap
```

Use separate Tilemaps when sorting, collision, or editing responsibilities differ. Decorative tiles should not automatically receive collision.

## Setup
1. Create a Grid and Tilemap using the 2D Tilemap menu.
2. Open Window → 2D → Tile Palette and create/select a palette.
3. Add sprites as tiles and paint the map.
4. Add TilemapCollider2D to the collision tilemap only.
5. If merging adjacent shapes is useful, add a CompositeCollider2D and configure the TilemapCollider2D to contribute to it; verify the required Rigidbody2D and composite settings for your Unity version.
6. Set the collision tilemap's Layer and check Project Settings → Physics 2D.
7. Test corners, ledges, and narrow passages in Play mode.

## Collider choices
- TilemapCollider2D creates collision geometry from tiles.
- CompositeCollider2D can combine compatible shapes, reducing seams and collider count.
- Rigidbody2D is commonly configured as Static for non-moving level geometry, depending on the composite setup.

## Common issues
- Collider is on the wrong Tilemap.
- Decorative tilemap accidentally blocks the player.
- Physics layer matrix prevents contact.
- Composite setup is incomplete or not generating geometry.
- Tile sprite pivot or cell size makes visuals look misaligned.

Reference: [Tilemap Collider 2D](https://docs.unity3d.com/Manual/class-TilemapCollider2D.html)
