# Vector Math Reference for 3D Games

## Position versus direction
A position describes a location. A direction describes an offset/orientation. Subtracting positions gives a direction:

    Vector3 direction = target.position - transform.position;
    float distance = direction.magnitude;
    Vector3 unitDirection = direction.normalized;

Use sqrMagnitude for comparisons when distance itself is not needed:

    if ((target.position - transform.position).sqrMagnitude < range * range)
    {
        // Target is within range.
    }

## Useful methods
- Vector3.Distance(a, b): distance between points.
- Vector3.Dot(a, b): alignment test; useful for checking whether a target is in front.
- Vector3.Cross(a, b): perpendicular vector.
- Vector3.ProjectOnPlane(v, normal): remove a component perpendicular to a plane.
- Vector3.MoveTowards(current, target, maxDistanceDelta): bounded step.
- Vector3.Lerp(a, b, t): interpolation between values.
- Quaternion.LookRotation(direction): face a direction.
- Quaternion.Slerp(a, b, t): interpolate rotations.
- Mathf.Clamp(value, min, max): constrain a number.
- Mathf.Atan2(y, x): angle from components, useful for 2D aiming.

## Camera-relative movement
For ground-plane movement, camera forward includes pitch. Project it onto the horizontal plane:

    Vector3 forward = Vector3.ProjectOnPlane(camera.forward, Vector3.up).normalized;
    Vector3 right = Vector3.ProjectOnPlane(camera.right, Vector3.up).normalized;
    Vector3 move = forward * input.y + right * input.x;
    move = Vector3.ClampMagnitude(move, 1f);

Clamping magnitude prevents diagonal movement from being faster than straight movement.

## Dot product and field of view
For normalized forward and target direction, dot is 1 when aligned, 0 at 90 degrees, and -1 when opposite. A threshold can make a simple cone test. Use a raycast if physical line of sight matters.

## Lerp versus MoveTowards
Lerp interpolates by a fraction; repeated small fractions approach the target asymptotically. MoveTowards moves by a bounded distance. SmoothDamp is useful for damped movement. Constant-speed movement is often better expressed with MoveTowards or velocity.

## Common mistakes
- Normalizing a vector whose magnitude is still needed.
- Adding positions where a direction was intended.
- Comparing magnitude when squared magnitude suffices.
- Passing a zero direction into LookRotation.
- Multiplying mouse delta by deltaTime without considering how the input value is defined.
