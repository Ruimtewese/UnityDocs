# 3D Model Import Settings Reference

Select a model asset in the Project window to view model import settings. Available options vary by source file and Unity version.

## Model tab
- Scale Factor: converts source-unit size into Unity world units. Inspect real-world dimensions before changing it.
- Convert Units: may normalize source units depending on the importer/version.
- Import BlendShapes: required for meshes using blend shapes.
- Read/Write: stores an accessible CPU copy of mesh data and increases memory use; enable only when scripts or features need it.
- Mesh Compression: reduces storage/memory but may affect precision.
- Generate Colliders: can create import-derived collider setup, but simple hand-authored colliders are often better for gameplay.

## Rig tab
Choose Animation Type to match the asset (for example, Humanoid or Generic). Humanoid rigs require valid mapping/Avatar configuration. Check bone mapping and Avatar validity when retargeted animation fails.

## Animation tab
Review imported clip ranges, loop settings, root transform rotation/position options, and events. Incorrect clip ranges can create empty animations or include neighboring actions. Loop Time should match the clip behavior.

## Materials tab
Imported materials and textures may be extracted or remapped. Verify shader compatibility with Built-in/URP/HDRP and inspect texture assignments. Pink materials often point to shader/pipeline incompatibility.

## Troubleshooting
- Model too large/small: inspect source units and scale factor.
- Mesh looks inside-out or black: inspect normals, material shader, lighting, and import settings.
- Missing animation: inspect Rig type, clip ranges, Avatar setup, and Animator state.
- Runtime mesh modification fails: verify Read/Write and feature requirements.
- Materials turn pink after pipeline migration: convert/remap materials for the active render pipeline.
