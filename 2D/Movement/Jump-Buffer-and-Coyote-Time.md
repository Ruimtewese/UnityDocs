# 2D Platformer: Jump Buffering and Coyote Time

Jump buffering remembers a jump press briefly before landing. Coyote time allows a jump shortly after walking off an edge. Both make platform controls more forgiving.

## Concept
- Jump buffer: start a short timer when Jump is pressed; allow the jump if the character becomes grounded before it expires.
- Coyote time: refresh a timer while grounded; allow jumping while it remains above zero after leaving ground.
- Clear timers deliberately after a successful jump so an old press doesn't trigger later.

## Integration pattern
This component shows the timer logic. Call SetJumpPressed from an input callback and SetGrounded from your grounded detector. Your movement controller should consume CanJump and apply its own velocity/force.

    using UnityEngine;

    public sealed class JumpGrace2D : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float bufferDuration = 0.12f;
        [SerializeField, Min(0f)] private float coyoteDuration = 0.1f;
        private float jumpBufferRemaining;
        private float coyoteRemaining;
        public bool CanJump => jumpBufferRemaining > 0f && coyoteRemaining > 0f;

        public void SetJumpPressed() => jumpBufferRemaining = bufferDuration;

        public void SetGrounded(bool grounded)
        {
            if (grounded) coyoteRemaining = coyoteDuration;
        }

        private void Update()
        {
            jumpBufferRemaining = Mathf.Max(0f, jumpBufferRemaining - Time.deltaTime);
            coyoteRemaining = Mathf.Max(0f, coyoteRemaining - Time.deltaTime);
        }

        public bool ConsumeJump()
        {
            if (!CanJump) return false;
            jumpBufferRemaining = 0f;
            coyoteRemaining = 0f;
            return true;
        }
    }

Use Time.deltaTime for gameplay timers that follow scaled time. If the game can pause, ensure the jump logic does not run unexpectedly while paused. Keep grounded detection in the controller/ground-check system so there is one authoritative grounded state.

## Common mistakes
- Grounded is not refreshed every physics/frame step.
- Jump input is never cleared after use.
- Timers use Update while the controller reads/clears them from a different script with unclear ordering.
- Coyote time is refreshed while airborne due to a faulty ground check.
