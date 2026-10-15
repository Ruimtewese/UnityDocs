# 3D: Debugging and Profiling

## Guide
- [Debugging and profiling](./Debugging-and-Profiling.md)

## Workflow
Read the first Console error, open the source line, inspect scene references and component states, then reproduce with the smallest scene. Validate package/version support for build failures. Use Gizmos and Debug.DrawRay to inspect query origins and hit directions.

## Profile rather than guess
Use Window → Analysis → Profiler and test Development Builds on target hardware. Separate CPU, GPU, memory, garbage collection, physics, rendering, AI, and audio costs. Make one change at a time and profile again.

## Common diagnosis
- Null reference: missing object/reference or initialization ordering.
- No raycast hit: wrong mask, origin inside geometry, range too short, or trigger filtering.
- Rigidbody jitter: conflicting Transform writes or poor physics interpolation setup.
- NavMesh movement failure: invalid path, missing bake, or agent off mesh.
- Render problem: culling mask, clip planes, shader/pipeline mismatch, or disabled renderer.
