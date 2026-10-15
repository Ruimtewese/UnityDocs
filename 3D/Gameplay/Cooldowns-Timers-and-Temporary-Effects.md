# 3D Cooldowns, Timers, and Temporary Effects

## Cooldown pattern
Use a timestamp for a simple cooldown rather than a coroutine if you only need to check whether an action is ready.

    using UnityEngine;

    public sealed class CooldownGate : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float cooldown = 1f;
        private float nextAllowedTime;

        public bool TryUse()
        {
            if (Time.time < nextAllowedTime) return false;
            nextAllowedTime = Time.time + cooldown;
            return true;
        }

This cooldown uses scaled game time, so it pauses at timeScale zero. Use Time.unscaledTime if it must continue during pause.

## Temporary effect with coroutine
Use a Coroutine when an effect needs a start, wait, and cleanup sequence. Ensure cleanup happens if the object is disabled/destroyed; for robust effects, centralize their ownership and cancellation.

## Timer choices
- Time.time / Time.deltaTime: scaled gameplay time.
- Time.unscaledTime / Time.unscaledDeltaTime: real elapsed application time unaffected by timeScale.
- WaitForSeconds: scaled coroutine delay.
- WaitForSecondsRealtime: unscaled coroutine delay.
- FixedUpdate / fixedDeltaTime: physics-step timing.

## Common mistakes
- Cooldown initialized to zero but current scaled time assumptions differ.
- Timer is reset by every repeated input event.
- Pause state stops a timer that was meant to be real-time.
- Multiple scripts independently control the same temporary state.
