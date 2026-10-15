# 3D Jumping and Ground Checks

This guide uses CharacterController. Unlike Rigidbody, CharacterController is moved with CharacterController.Move and does not receive gravity automatically.

## Controller with jump
Save as CharacterJump3D.cs and attach to an object with CharacterController. Feed MoveInput and JumpInput from your input system.

    using UnityEngine;

    [RequireComponent(typeof(CharacterController))]
    public sealed class CharacterJump3D : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float moveSpeed = 5f;
        [SerializeField, Min(0f)] private float jumpHeight = 1.5f;
        [SerializeField] private float gravity = -20f;
        [SerializeField] private Transform cameraTransform;
        private CharacterController controller;
        private Vector2 moveInput;
        private bool jumpRequested;
        private float verticalVelocity;

        private void Awake() => controller = GetComponent<CharacterController>();

        public void SetMoveInput(Vector2 input) => moveInput = Vector2.ClampMagnitude(input, 1f);
        public void RequestJump() => jumpRequested = true;

        private void Update()
        {
            Vector3 forward = cameraTransform != null
                ? Vector3.ProjectOnPlane(cameraTransform.forward, Vector3.up).normalized
                : Vector3.forward;
            Vector3 right = cameraTransform != null
                ? Vector3.ProjectOnPlane(cameraTransform.right, Vector3.up).normalized
                : Vector3.right;
            Vector3 horizontal = (forward * moveInput.y + right * moveInput.x) * moveSpeed;

            if (controller.isGrounded && verticalVelocity < 0f)
                verticalVelocity = -2f;

            if (jumpRequested && controller.isGrounded)
                verticalVelocity = Mathf.Sqrt(jumpHeight * -2f * gravity);
            jumpRequested = false;

            verticalVelocity += gravity * Time.deltaTime;
            Vector3 velocity = horizontal + Vector3.up * verticalVelocity;
            controller.Move(velocity * Time.deltaTime);
        }
    }

The input layer should call SetMoveInput continuously and RequestJump when pressed. Jump is consumed each frame; if you need jump buffering before landing, keep a timed buffer rather than clearing it immediately. Make sure the scene has walkable colliders and the controller capsule is correctly sized.

## Ground-check details
CharacterController.isGrounded reports whether the controller was grounded during the latest Move call; after jumping or moving across ledges, use it with care. A dedicated SphereCast/CheckSphere ground probe can help with slopes and custom grounding rules, but must ignore the player's own collider.

## Common mistakes
- Gravity is not applied automatically by CharacterController.
- Forgetting to multiply velocity by deltaTime when calling Move.
- Setting vertical velocity to zero every frame while grounded can erase a jump request.
- The camera transform is tilted down; project its forward/right vectors onto the horizontal plane for movement.
