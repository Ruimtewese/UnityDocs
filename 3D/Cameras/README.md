# 3D: Cameras

## Important camera settings
- **Projection:** Perspective or Orthographic.
- **Field of View:** vertical viewing angle in common perspective setups.
- **Clipping Planes:** Near and Far control visible depth range.
- **Culling Mask:** determines which layers the camera renders.
- **Depth/priority and viewport settings:** affect camera composition and ordering depending on project setup.

## Follow and orbit cameras
For a simple rig, separate camera position, look direction, and target. Cinemachine provides reusable follow/look-at rigs, blends, framing, and other camera behaviours through its package. Third-person cameras need obstacle handling so they do not pass through walls.

## Common mistakes
Too-small near clipping distances can worsen depth precision. Excessive smoothing causes lag. Player facing and camera facing are not automatically the same. Test different aspect ratios and control devices.

Reference: [Camera API](https://docs.unity3d.com/ScriptReference/Camera.html)
