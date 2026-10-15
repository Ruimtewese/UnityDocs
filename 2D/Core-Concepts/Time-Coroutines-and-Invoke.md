# Unity Time, Coroutines, and Invoke

## Time values
- Time.deltaTime: elapsed scaled time since the previous rendered frame. Use for frame-based movement.
- Time.fixedDeltaTime: fixed physics timestep. Use when computing per-physics-step motion.
- Time.time: scaled game time; stops advancing when timeScale is zero.
- Time.unscaledDeltaTime: frame time unaffected by timeScale; useful for pause UI.
- Time.timeScale: global scaled-time multiplier. Zero pauses scaled time, not every Unity system.

## Frame-rate-independent movement
For an object moved directly by Transform rather than a Rigidbody:

    transform.position += direction.normalized * speed * Time.deltaTime;

Without deltaTime, distance changes with frame rate. For Rigidbody movement, use its physics API in FixedUpdate instead of combining Transform movement and simulation.

## Coroutine example
Save as TimedEffect.cs.

    using System.Collections;
    using UnityEngine;

    public sealed class TimedEffect : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float duration = 1f;
        private Coroutine activeRoutine;

        public void Begin()
        {
            if (activeRoutine != null) StopCoroutine(activeRoutine);
            activeRoutine = StartCoroutine(Run());
        }

        private IEnumerator Run()
        {
            // Apply the effect here.
            yield return new WaitForSeconds(duration);
            // Remove the effect here.
            activeRoutine = null;
        }

        private void OnDisable()
        {
            if (activeRoutine != null) StopCoroutine(activeRoutine);
            activeRoutine = null;
        }
    }

WaitForSeconds uses scaled time. Use WaitForSecondsRealtime for real time even while paused. Coroutines are not threads; blocking work inside one still blocks the main thread.

## Invoke and cancellation
Invoke(nameof(MethodName), delay) schedules a delayed method; CancelInvoke cancels pending Invoke calls on that component. For sequences, cancellation, or data passed between steps, coroutines are often clearer.
