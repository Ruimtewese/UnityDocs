# 2D Recipe: Camera Shake

A screen shake should usually be an additive offset on a dedicated camera rig, not a second script that competes to set the camera's main position. This example uses unscaled time so the shake can run during a pause; change that deliberately if desired.

## Shake component
Attach to a camera rig child, or to a transform whose base position is owned by another camera-follow system. Call Shake from a gameplay event.

    using System.Collections;
    using UnityEngine;

    public sealed class CameraShake2D : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float defaultDuration = 0.15f;
        [SerializeField, Min(0f)] private float defaultMagnitude = 0.15f;
        private Vector3 baseLocalPosition;
        private Coroutine activeShake;

        private void Awake() => baseLocalPosition = transform.localPosition;

        public void Shake()
        {
            Shake(defaultDuration, defaultMagnitude);
        }

        public void Shake(float duration, float magnitude)
        {
            if (activeShake != null) StopCoroutine(activeShake);
            activeShake = StartCoroutine(ShakeRoutine(duration, magnitude));
        }

        private IEnumerator ShakeRoutine(float duration, float magnitude)
        {
            float elapsed = 0f;
            while (elapsed < duration)
            {
                Vector2 offset = Random.insideUnitCircle * magnitude;
                transform.localPosition = baseLocalPosition + new Vector3(offset.x, offset.y, 0f);
                elapsed += Time.unscaledDeltaTime;
                yield return null;
            }
            transform.localPosition = baseLocalPosition;
            activeShake = null;
        }

        private void OnDisable()
        {
            if (activeShake != null) StopCoroutine(activeShake);
            transform.localPosition = baseLocalPosition;
            activeShake = null;
        }
    }

This component assumes the base local position stays fixed. For a camera that follows a moving target, place the shake offset on a dedicated child rig or integrate shake into the camera controller so follow updates don't overwrite it.

## Common mistakes
- Camera follow and shake scripts both write the same Transform.
- Shake routine ends without restoring the original position.
- Shake coroutine is disabled while active and leaves a residual offset.
- Magnitude is too high and makes the game difficult to play.
