# 2D Coordinate Spaces and Pixels Per Unit

## World space and screen space
World coordinates describe the game scene. Screen coordinates describe pixel positions in the game window. UI coordinates may use a Canvas-specific space. Convert between them explicitly.

```csharp
Vector3 screen = Input.mousePosition;
Vector3 world = Camera.main.ScreenToWorldPoint(
    new Vector3(screen.x, screen.y, -Camera.main.transform.position.z));
```

This example assumes a 2D scene on the XY plane with a camera looking along Z. For production code, cache a camera reference instead of repeatedly using Camera.main, and account for the camera's orientation and desired depth.

## Pixels Per Unit (PPU)
PPU specifies how many source-image pixels represent one world unit. For example, a 32-pixel-wide sprite imported at 32 PPU is approximately one world unit wide before Transform scaling.

Consistency matters more than a particular PPU number. If some assets use 16 PPU and others use 100 PPU, they may appear at very different sizes unless their import settings and Transform scales are planned.

## Pixels and filtering
- **Point:** preserves crisp pixel boundaries, often preferred for pixel art.
- **Bilinear:** smooths texture sampling.
- **Compression:** may reduce memory but can introduce artifacts.
- **Mip Maps:** commonly useful for textures viewed at varying scales; evaluate their suitability for 2D UI and pixel art.

## Camera sizing
An orthographic camera's Size is half the vertical view span in world units. Horizontal span depends on the aspect ratio. Test several window shapes, especially if the game uses a fixed pixel-art resolution.

## Pixel-perfect checklist
1. Consistent PPU across the art set.
2. Point filtering when appropriate.
3. Deliberate reference resolution.
4. Camera movement aligned with the chosen pixel grid if required.
5. Test at target resolutions and aspect ratios.

Reference: [Sprite import settings](https://docs.unity3d.com/Manual/texture-type-sprite.html)
