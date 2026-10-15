# 2D Development

A practical reference for sprite-based games: platformers, top-down games, tile-based worlds, pixel art, and 2D action games.

## Topic index
- [Core Concepts](./Core-Concepts/README.md)
- [Components](./Components/README.md)
- [Movement](./Movement/README.md)
- [Physics](./Physics/README.md)
- [Input](./Input/README.md)
- [Animation](./Animation/README.md)
- [Cameras](./Cameras/README.md)
- [Tilemaps](./Tilemaps/README.md)
- [UI](./UI/README.md)
- [Audio](./Audio/README.md)
- [Rendering](./Rendering/README.md)
- [Optimization](./Optimization/README.md)
- [Recipes](./Recipes/README.md)

## Recommended learning order
1. Learn coordinate space, sprites, PPU, and sorting.
2. Understand SpriteRenderer, Rigidbody2D, and Collider2D.
3. Build movement and ground/wall detection.
4. Add input actions, animation, and a camera.
5. Build levels with Tilemaps and add UI/audio.
6. Profile and optimize only after the gameplay works.

## 2D-specific essentials
- X/Y are usually the gameplay plane; Z may still affect camera depth and transparency sorting.
- An orthographic camera does not use perspective scaling.
- Pixels Per Unit (PPU) maps source-image pixels to Unity world units.
- Use Rigidbody2D and Collider2D when gameplay needs 2D physics.
- Sorting Layers and Order in Layer commonly control sprite draw order.
- For pixel art, make deliberate choices about PPU, filtering, reference resolution, and camera movement.

Official references: [2D game development](https://docs.unity3d.com/Manual/Unity2D.html) · [Scripting API](https://docs.unity3d.com/ScriptReference/)
