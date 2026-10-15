# 3D Transforms and Essential Math

## Position spaces
- `transform.position`: world position.
- `transform.localPosition`: position relative to parent.
- `TransformPoint`: converts a local point to world space.
- `InverseTransformPoint`: converts a world point to local space.
- `TransformDirection`: transforms a direction while ignoring position.
- `InverseTransformDirection`: converts a world direction to local direction.

A child inherits the parent's transform. If a parent rotates, the child's world position and orientation can change even if the child's local values do not.

## Vector basics
- `Vector3.magnitude`: length of a vector.
- `Vector3.sqrMagnitude`: squared length; useful when only comparing distances.
- `Vector3.normalized`: direction with length one (or zero for a zero vector).
- `Vector3.Dot(a, b)`: alignment measure; useful for front/behind and angle checks.
- `Vector3.Cross(a, b)`: perpendicular vector in 3D.
- `Vector3.Distance(a, b)`: distance between points.

## Direction and rotation
```csharp
Vector3 toTarget = target.position - transform.position;
float distance = toTarget.magnitude;

if (distance > 0.001f)
{
    Vector3 direction = toTarget / distance;
    Quaternion desiredRotation = Quaternion.LookRotation(direction, Vector3.up);
    transform.rotation = Quaternion.Slerp(
        transform.rotation, desiredRotation, turnSpeed * Time.deltaTime);
}
```

Guard against a zero direction before calling LookRotation. The example assumes the character's forward axis is Z and uses world up.

## Time
- `Time.deltaTime`: elapsed time since the previous rendered frame; use for frame-rate-independent per-frame movement.
- `Time.fixedDeltaTime`: configured physics step duration.
- `Time.time`: scaled time since play began.
- `Time.unscaledDeltaTime`: elapsed frame time unaffected by time scale.

## Common mistakes
- Comparing magnitudes when squared magnitudes are enough.
- Forgetting to multiply a per-second speed by elapsed time.
- Confusing a direction vector with a point.
- Using Euler angles for repeated incremental 3D rotation when quaternion APIs are more suitable.
- Assuming local and world values are interchangeable.

References: [Transform](https://docs.unity3d.com/ScriptReference/Transform.html) · [Vector3](https://docs.unity3d.com/ScriptReference/Vector3.html) · [Quaternion](https://docs.unity3d.com/ScriptReference/Quaternion.html)
