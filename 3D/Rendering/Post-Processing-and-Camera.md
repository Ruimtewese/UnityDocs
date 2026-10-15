# 3D Rendering: Camera and Post-Processing Basics

## Camera checklist
- Projection: Perspective for normal 3D depth; Orthographic for fixed-size projection.
- Field of View: wider values show more area but distort perspective more at the edges.
- Clipping Planes: near should be no closer than necessary; far should cover required world depth. Extreme ranges can reduce depth-buffer precision.
- Culling Mask: controls which GameObject layers the camera renders.
- Depth/stacking: camera order and pipeline-specific stacking can alter render output.
- Target Display and viewport rect: useful for multi-camera and split-screen setups.

## URP post-processing overview
In a URP project, post-processing often uses a Global Volume with a Volume Profile and a camera configured to render post-processing. Exact inspector labels can vary between package versions. Add only effects that improve the desired art direction, such as color adjustments, bloom, vignette, or depth of field.

## Troubleshooting
If a scene is pink, investigate shader/render-pipeline compatibility rather than post-processing first. If objects vanish, verify layer culling, clipping planes, camera orientation, and renderer enabled state. If the camera looks washed out, review exposure, color space, lighting, and volume overrides.

## Performance
Post-processing can add GPU cost, particularly at high resolution. Profile a target build. Avoid stacking multiple cameras and full-screen effects without need.
