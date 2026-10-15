# 3D Camera-Relative Rigidbody Movement

Use this when a Rigidbody character should move relative to the camera's horizontal facing direction. Add a Rigidbody and CapsuleCollider to the player, freeze unwanted rotation axes, and assign an optional camera Transform. Feed movement from the Input System layer.

## Controller
Save as RigidbodyPlayerMovement3D.cs.

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody))]
    public sealed class RigidbodyPlayerMovement3D : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float moveSpeed = 5f;
        [SerializeField, Min(0f)] private float turnSpeed = 720f;
        [SerializeField] private Transform cameraTransform;
        private Rigidbody body;
        private Vector2 moveInput;

        private void Awake() => body = GetComponent<Rigidbody>();

        public void SetMoveInput(Vector2 input)
        {
            moveInput = Vector2.ClampMagnitude(input, 1f);
        }

        private void FixedUpdate()
        {
            Vector3 forward = cameraTransform != null
                ? Vector3.ProjectOnPlane(cameraTransform.forward, Vector3.up).normalized
                : Vector3.forward;
            Vector3 right = cameraTransform != null
                ? Vector3.ProjectOnPlane(cameraTransform.right, Vector3.up).normalized
                : Vector3.right;
            Vector3 desired = forward * moveInput.y + right * moveInput.x;
            desired = Vector3.ClampMagnitude(desired, 1f);

            Vector3 velocity = body.linearVelocity;
            body.linearVelocity = new Vector3(
                desired.x * moveSpeed, velocity.y, desired.z * moveSpeed);

            if (desired.sqrMagnitude > 0.001f)
            {
                Quaternion target = Quaternion.LookRotation(desired, Vector3.up);
                body.MoveRotation(Quaternion.RotateTowards(
                    body.rotation, target, turnSpeed * Time.fixedDeltaTime));
            }
        }
    }

Set Rigidbody interpolation to Interpolate if needed and test Collision Detection settings for fast motion. This controller directly writes horizontal velocity; it intentionally preserves vertical velocity so gravity and jumping still work. Do not combine with another script that writes the same Rigidbody velocity without an explicit movement ownership design.

## Common mistakes
- Camera forward includes pitch, so movement tilts into the ground; project it onto the horizontal plane.
- A second script overwrites velocity or rotation.
- Rigidbody rotation is not constrained, so collisions tip the player over.
- Input is never passed into SetMoveInput.
- Normalization is omitted, making diagonal movement faster.
