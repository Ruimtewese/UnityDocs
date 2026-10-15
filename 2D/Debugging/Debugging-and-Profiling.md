# Debugging, Testing, and Profiling

## Debugging sequence
1. Read the first Console error; later errors may be consequences.
2. Double-click it to open the source line.
3. Check filename/class name, Inspector references, active scene, and prefab instance.
4. Reproduce in the smallest scene possible.
5. Log state changes rather than every frame.
6. Remove or gate noisy logs after fixing the issue.

    Debug.Log("Reached checkpoint", this);
    Debug.LogWarning("Target has not been assigned.", this);
    Debug.LogError("Required component is missing.", this);

Passing this as context makes Console messages clickable.

## Physics debugging
- Enable Gizmos in Scene view.
- Draw casts with Debug.DrawRay or Debug.DrawLine.
- Verify layer masks, collider sizes/offsets, body type, constraints, and collision matrix.
- Check trigger callback Rigidbody requirements.
- Check whether the collider is on a child while the script checks only the root.

## Profiler
Open Window → Analysis → Profiler. Profile a Development Build on target hardware when possible; Editor overhead changes results. Measure CPU, GPU, memory, garbage collection, rendering, and physics separately.

## Frequent failures
NullReferenceException: missing reference, destroyed object, or initialization-order issue. Missing script: class/file mismatch or deleted/renamed class. Input not detected: wrong map/backend/binding or focus. Physics jitter: Transform writes conflict with Rigidbody simulation. Build-only failure: missing scene, case mismatch, platform API, unsupported package, or Editor-only code.
