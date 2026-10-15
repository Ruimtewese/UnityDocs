# 3D Debugging and Profiling

## First-error workflow
1. Read the first Console error.
2. Open the indicated source line.
3. Check object references, active state, component dependencies, and input actions.
4. Reproduce in a minimal scene.
5. Verify build-specific and platform-specific behavior.
6. Remove temporary logs after fixing the cause.

## Physics visualization
Draw rays using Debug.DrawRay and hit points using Gizmos or debug lines. Inspect collider bounds, Rigidbody interpolation, collision mode, layers, and the collision matrix. When a CharacterController sticks, inspect capsule center/height, skin width, slope limit, step offset, and scene geometry.

## Profiler workflow
Open Window → Analysis → Profiler. Use a Development Build on target hardware for realistic results. Inspect CPU, GPU, memory, garbage collection, rendering, physics, and audio. A slow Editor frame may not reflect standalone build performance.

## Common errors
- NullReferenceException: reference is unassigned, destroyed, or not initialized in time.
- MissingReferenceException: a Unity object was destroyed but code still holds a reference.
- Physics jitter: mixed Transform and Rigidbody movement, or unsuitable interpolation/timestep.
- Mesh/material invisible: culling mask, clipping planes, renderer state, shader, or missing material.
- Animation not driving movement: Animator state/parameters/root-motion setup conflict.
- NavMeshAgent doesn't move: agent isn't on a baked NavMesh, path is invalid, or another script conflicts.
- Build fails while Editor runs: unsupported API/package, missing scene/content, or Editor-only code included in runtime.

## Profiling priorities
Measure before optimizing. Reduce allocations, expensive physics operations, excessive real-time lighting, unnecessary object churn, repeated scene-wide searches, and over-frequent AI updates only when profiling shows those systems are costly.
