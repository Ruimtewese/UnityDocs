# 3D Math and Quaternion Recipes

## Rotate toward a target
Compute direction from the current position to the target, then create a target rotation. Handle the zero direction before calling LookRotation.

    Vector3 direction = target.position - transform.position;
    if (direction.sqrMagnitude > 0.0001f)
    {
        Quaternion targetRotation = Quaternion.LookRotation(direction, Vector3.up);
        transform.rotation = Quaternion.RotateTowards(
            transform.rotation, targetRotation, turnSpeed * Time.deltaTime);
    }

For Rigidbody rotation, use the Rigidbody API at the appropriate physics time rather than directly assigning Transform rotation.

## Angles
- Vector3.Angle(a, b): unsigned angle between two vectors.
- Mathf.DeltaAngle(current, target): shortest signed difference between angles.
- Mathf.Atan2(y, x): angle from two coordinates.
- Quaternion.Euler(x, y, z): construct a rotation from Euler angles.
- Quaternion.Inverse(q): inverse rotation.
- Quaternion.RotateTowards(a, b, maxDegreesDelta): bounded rotation step.

Euler angles are convenient to inspect but can wrap around and lead to unintuitive interpolation. Quaternions are the usual representation for interpolating rotations.

## Convert between spaces

    Vector3 worldPoint = transform.TransformPoint(localPoint);
    Vector3 localPoint = transform.InverseTransformPoint(worldPoint);
    Vector3 worldDirection = transform.TransformDirection(localDirection);
    Vector3 localDirection = transform.InverseTransformDirection(worldDirection);

Point conversions include position and scale; direction conversions are for orientation/direction and differ from point conversions.

## Screen and world
Camera.ScreenToWorldPoint converts a screen point using camera-space depth; its input z is a distance from the camera, not the desired world Z. Camera.WorldToScreenPoint converts a world point to pixels. Camera.ScreenPointToRay creates a world ray through a screen pixel; use ViewportPointToRay for normalized viewport coordinates.

## Common mistakes
- Confusing local and world coordinate values.
- Assuming ScreenToWorldPoint input z is a world-space Z.
- Applying yaw/pitch on the wrong Transform.
- Interpolating Euler angles directly when an angular wrap occurs.
- Calling LookRotation with a zero direction.
