# 2D: Rendering

## Sorting and materials
Sorting Layers define broad draw groups; Order in Layer orders renderers within them. Transparency sorting settings and material/shader configuration can also affect draw order and appearance.

## URP 2D Renderer
URP's 2D Renderer can support 2D lights and shadows when the project is configured for it. Sprites need compatible materials/shaders to respond to the chosen lighting setup. Built-in Render Pipeline and URP workflows differ.

## Debug checklist
1. Confirm the active render pipeline and renderer asset.
2. Inspect SpriteRenderer material and sorting settings.
3. Check camera culling mask and object layers.
4. Verify 2D Light settings and target sorting layers.
5. Test at the actual target resolution.

Reference: [URP documentation](https://docs.unity3d.com/Manual/urp/2DIndex.html)
