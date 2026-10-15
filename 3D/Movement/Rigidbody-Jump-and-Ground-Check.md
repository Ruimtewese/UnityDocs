# 3D Rigidbody Jump and Ground Check

A Rigidbody character should be grounded using a query or collision state, then jump by setting the upward component of velocity or applying an impulse. Choose one approach and tune it consistently.

## Setup
Add a Rigidbody and CapsuleCollider to the player. Assign a child Transform near the feet and a LayerMask containing walkable ground. Freeze rotation axes if the character must remain upright. Do not use a CharacterController and Rigidbody movement controller on the same object without a deliberate design.

## Ground and jump controller
Save as RigidbodyJump3D.cs. Call SetJumpPressed from the input layer.

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody))]
    public sealed class RigidbodyJump3D : MonoBehaviour
    {
        [SerializeField] private Transform groundCheck;
        [SerializeField, Min(0.01f)] private float groundRadius = 0.18f;
        [SerializeField] private LayerMask groundLayers;
        [SerializeField, Min(0f)] private float jumpVelocity = 6f;
        private Rigidbody body;
        private bool jumpQueued;
        public bool IsGrounded { get; private set; }

        private void Awake() => body = GetComponent<Rigidbody>();

        public void SetJumpPressed() => jumpQueued = true;

        private void FixedUpdate()
        {
            IsGrounded = groundCheck != null &&
                Physics.CheckSphere(groundCheck.position, groundRadius, groundLayers,
                    QueryTriggerInteraction.Ignore);

            if (IsGrounded && jumpQueued)
            {
                Vector3 velocity = body.linearVelocity;
                velocity.y = jumpVelocity;
                body.linearVelocity = velocity;
                jumpQueued = false;
            }

            if (!IsGrounded && jumpQueued)
                jumpQueued = false;
        }

        private void OnDrawGizmosSelected()
        {
            if (groundCheck == null) return;
            Gizmos.color = Color.yellow;
            Gizmos.DrawWireSphere(groundCheck.position, groundRadius);
        }
    }

This simple sample does not buffer jumps while airborne. Add a short timer if coyote time/jump buffering is required. Put groundCheck just below the capsule feet, and exclude the player's layer from groundLayers. Tune the radius to avoid false grounded states near walls.

## Common mistakes
- Ground mask includes player colliders.
- Probe sits too high and reports grounded while falling beside a wall.
- Jump velocity is set every physics step while jump input remains held.
- Rigidbody rotation tips the character.
- Physics query is run in a different coordinate space than the Transform setup.
