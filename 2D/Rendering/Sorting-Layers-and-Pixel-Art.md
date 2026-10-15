# 2D Rendering: Sorting Layers, Sprite Import, and Pixel Art

## Sorting rules
SpriteRenderer has a Sorting Layer and Order in Layer. Sorting Layers are ordered in the Tags and Layers settings; within a layer, larger order values render in front of smaller ones. TilemapRenderer also exposes sorting controls. Do not confuse a render Sorting Layer with a GameObject physics Layer.

## Sprite import checklist
- Texture Type: Sprite (2D and UI).
- Sprite Mode: Single for one image or Multiple for a sheet.
- Pixels Per Unit: keep consistent across assets designed at the same world scale.
- Filter Mode: Point for crisp pixel art; Bilinear often suits smooth artwork.
- Compression: select based on target quality and memory constraints.
- Sprite Editor: slice the sheet and confirm pivots and borders.

## Pixel-perfect camera
For pixel art, use consistent PPU, matching art/reference-resolution choices, and the 2D Pixel Perfect package if appropriate. Test camera movement at actual target resolutions. Fractional positions/scales can create blurry or uneven pixel steps.

## Top-down Y sorting
In a top-down game, objects nearer the bottom of the screen often need to draw in front of objects farther up. Configure the SpriteRenderer's mode appropriately for the project, or assign order based on a consistent Y-sorting rule. If multiple renderers are on one character, keep their ordering coordinated.

## Transparency and render order
Transparent sprites can cover one another according to sorting settings rather than depth-writing like opaque geometry. Use the Frame Debugger and Scene view to diagnose unexpected draw order.

Common mistakes: mismatched Sorting Layer names, two scripts fighting over order, wrong pivots causing visual jumps, incorrect PPU, and physics Layer changes used to try to fix visual sorting.
