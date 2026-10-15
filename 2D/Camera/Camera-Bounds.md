# 2D Camera Bounds and Level Confinement

A follow camera should stay inside level bounds. A simple clamp computes the camera's orthographic half-height and half-width, then limits its center position so the view stays inside the rectangle.

## Clamp an orthographic camera
Attach to the Camera or camera rig. Assign a target and world-space minimum/maximum bounds. Assumes a 2D orthographic camera and rectangular level bounds.

    using UnityEngine;

    [RequireComponent(typeof(Camera))]
    public sealed class CameraBounds2D : MonoBehaviour
    {
        [SerializeField] private Transform target;
        [SerializeField] private Vector2 minBounds;
        [SerializeField] private Vector2 maxBounds;
        [SerializeField] private Vector2 offset;
        private Camera cam;

        private void Awake() => cam = GetComponent<Camera>();

        private void LateUpdate()
        {
            if (target == null || !cam.orthographic) return;

            float halfHeight = cam.orthographicSize;
            float halfWidth = halfHeight * cam.aspect;

            float minX = minBounds.x + halfWidth;
            float maxX = maxBounds.x - halfWidth;
            float minY = minBounds.y + halfHeight;
            float maxY = maxBounds.y - halfHeight;

            // When the view is wider/taller than the level, center it on that axis.
            float x = minX > maxX ? (minBounds.x + maxBounds.x) * 0.5f
                                  : Mathf.Clamp(target.position.x + offset.x, minX, maxX);
            float y = minY > maxY ? (minBounds.y + maxBounds.y) * 0.5f
                                  : Mathf.Clamp(target.position.y + offset.y, minY, maxY);

            transform.position = new Vector3(x, y, transform.position.z);
        }
    }

Set min/max to the actual world-space rectangle around the playable level, not the player's bounds. If using a separate smooth follow script, combine follow and clamping in one controller or ensure update order is intentional. Cinemachine Confiner may be preferable for irregular level boundaries.

## Common mistakes
- Using pixel bounds instead of world coordinates.
- Forgetting that camera half-width depends on aspect ratio.
- Clamping before smoothing, then smoothing moves the camera outside the bounds.
- The level is smaller than the camera view, requiring centering rather than an impossible clamp.
