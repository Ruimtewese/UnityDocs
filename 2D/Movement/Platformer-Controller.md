# 2D Platformer Controller — Complete Setup

This controller uses the Input System package and a Dynamic Rigidbody2D. It includes horizontal movement, ground checking, jump buffering, and coyote time. It is a foundation; slopes, ladders, wall jumps, variable jump height, and moving platform carry are separate features.

## Scene setup
1. Create a Player with SpriteRenderer, Rigidbody2D, and a CapsuleCollider2D or BoxCollider2D.
2. Set Rigidbody2D Body Type to Dynamic and Freeze Rotation Z.
3. Choose Interpolate if rendered motion looks jittery; choose collision detection based on speed.
4. Create a Ground layer and assign it to solid ground colliders.
5. Add a child Transform called GroundCheck, positioned just below the player's feet.
6. Install Input System and create a Gameplay map with Move (Value / Vector2) and Jump (Button).
7. Assign the actions to the script. The groundLayers mask must include ground and exclude the player layer.

## Script: PlatformerController2D.cs

    using UnityEngine;
    using UnityEngine.InputSystem;

    [RequireComponent(typeof(Rigidbody2D))]
    public sealed class PlatformerController2D : MonoBehaviour
    {
        [Header("Movement")]
        [SerializeField, Min(0f)] private float moveSpeed = 7f;
        [SerializeField, Min(0f)] private float jumpVelocity = 13f;

        [Header("Ground check")]
        [SerializeField] private Transform groundCheck;
        [SerializeField, Min(0.01f)] private float groundRadius = 0.12f;
        [SerializeField] private LayerMask groundLayers;

        [Header("Jump forgiveness")]
        [SerializeField, Min(0f)] private float jumpBufferTime = 0.12f;
        [SerializeField, Min(0f)] private float coyoteTime = 0.1f;

        [Header("Input actions")]
        [SerializeField] private InputActionReference moveAction;
        [SerializeField] private InputActionReference jumpAction;

        private Rigidbody2D body;
        private float horizontal;
        private float jumpBufferRemaining;
        private float coyoteRemaining;
        private bool grounded;

        private void Awake()
        {
            body = GetComponent<Rigidbody2D>();
        }

        private void OnEnable()
        {
            if (moveAction != null) moveAction.action.Enable();

            if (jumpAction != null)
            {
                jumpAction.action.performed += OnJump;
                jumpAction.action.Enable();
            }
        }

        private void OnDisable()
        {
            if (jumpAction != null)
            {
                jumpAction.action.performed -= OnJump;
                jumpAction.action.Disable();
            }

            if (moveAction != null) moveAction.action.Disable();
        }

        private void Update()
        {
            horizontal = moveAction != null
                ? moveAction.action.ReadValue<Vector2>().x
                : 0f;
        }

        private void FixedUpdate()
        {
            grounded = groundCheck != null &&
                Physics2D.OverlapCircle(groundCheck.position, groundRadius, groundLayers) != null;

            if (grounded)
                coyoteRemaining = coyoteTime;
            else
                coyoteRemaining = Mathf.Max(0f, coyoteRemaining - Time.fixedDeltaTime);

            Vector2 velocity = body.linearVelocity;
            velocity.x = horizontal * moveSpeed;

            if (jumpBufferRemaining > 0f && coyoteRemaining > 0f)
            {
                velocity.y = jumpVelocity;
                jumpBufferRemaining = 0f;
                coyoteRemaining = 0f;
            }
            else
            {
                jumpBufferRemaining = Mathf.Max(
                    0f, jumpBufferRemaining - Time.fixedDeltaTime);
            }

            body.linearVelocity = velocity;
        }

        private void OnJump(InputAction.CallbackContext context)
        {
            jumpBufferRemaining = jumpBufferTime;
        }

        private void OnDrawGizmosSelected()
        {
            if (groundCheck == null) return;
            Gizmos.color = Color.yellow;
            Gizmos.DrawWireSphere(groundCheck.position, groundRadius);
        }
    }

## How it works
- Update reads movement input; FixedUpdate checks ground and writes Rigidbody2D velocity.
- Jump callback starts a short buffer timer so a press just before landing can trigger.
- Ground contact refreshes coyote time; leaving an edge doesn't instantly remove jump eligibility.
- When both timers overlap, the controller applies jump velocity and consumes both timers.

## Important details
InputActionReference actions are enabled/disabled by this component. Avoid having multiple components independently enable/disable the same shared action. Jump buffer/coyote timers use fixed-step countdowns. This sample replaces horizontal velocity directly, so it does not implement acceleration or friction-based deceleration.

## Troubleshooting
- Never grounded: move GroundCheck just below feet and verify groundLayers.
- Jump not detected: check action binding, action type, reference assignment, and active input map.
- Diagonal movement irrelevant for platformer horizontal axis; only Move.x is used.
- Player tips: freeze Rigidbody2D rotation Z.
- Fast player tunnels through thin platforms: inspect continuous collision detection and timestep.
