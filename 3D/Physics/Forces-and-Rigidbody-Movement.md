# 3D Rigidbody Forces and Movement

## Choose the correct approach
- CharacterController: controllable character motion; gravity must be implemented in code.
- Rigidbody: physical interactions, forces, collision response, and momentum.
- Transform movement: suitable for intentionally non-physical objects, but it conflicts with physics if used on dynamic Rigidbody objects.

## Apply force
Attach this example to a Rigidbody object. Call Push from an input action or another gameplay event.

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody))]
    public sealed class RigidbodyPush3D : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float impulse = 5f;
        private Rigidbody body;

        private void Awake() => body = GetComponent<Rigidbody>();

        public void Push(Vector3 direction)
        {
            if (direction.sqrMagnitude < 0.0001f) return;
            body.AddForce(direction.normalized * impulse, ForceMode.Impulse);
        }
    }

## Force modes
- Force: continuous force over physics time.
- Acceleration: continuous acceleration independent of mass.
- Impulse: immediate momentum change influenced by mass.
- VelocityChange: immediate velocity change independent of mass.

Use AddForce from physics-aware code and select a mode that matches the desired behavior. For a precise arcade controller, setting selected velocity components may be more predictable, but should be the one movement system's responsibility.

## Rigidbody Inspector checklist
Use Gravity, Is Kinematic, Interpolate, Collision Detection, Constraints, Linear Damping, and Angular Damping deliberately. Fast bodies may need Continuous collision detection. Freeze unwanted rotation for upright characters. Interpolation can smooth rendered movement.

## Common mistakes
- AddForce is called every rendered Update when a continuous force should be applied in the physics step.
- Force mode is unsuitable for the desired response.
- Rigidbody's rotation tips the character.
- Dynamic Rigidbody is moved by direct Transform writes.
- Physics settings are tuned without checking body mass and target scale.
