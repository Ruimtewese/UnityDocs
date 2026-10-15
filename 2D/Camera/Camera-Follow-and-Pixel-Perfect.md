# 2D Camera: Follow, Framing, and Pixel Art

For an orthographic camera, Size is half the vertical world-space view height. Size 5 means 10 world units high; visible width depends on aspect ratio.

## Smooth follow
Save as CameraFollow2D.cs and attach to Main Camera. Assign the player Transform. A common convention is gameplay at Z = 0 and camera at Z = -10.

    using UnityEngine;
    public sealed class CameraFollow2D : MonoBehaviour
    {
        [SerializeField] private Transform target;
        [SerializeField, Min(0f)] private float smoothTime = 0.12f;
        [SerializeField] private Vector2 offset = new Vector2(0f, 1f);
        [SerializeField] private bool followX = true;
        [SerializeField] private bool followY = true;
        private Vector3 velocity;
        private void LateUpdate()
        {
            if (target == null) return;
            Vector3 current = transform.position;
            Vector3 desired = new Vector3(
                followX ? target.position.x + offset.x : current.x,
                followY ? target.position.y + offset.y : current.y,
                current.z);
            transform.position = Vector3.SmoothDamp(current, desired, ref velocity, smoothTime);
        }
    }

LateUpdate is a common place for camera following. For physics-driven targets, test interpolation and consider Cinemachine.

## Pixel art checklist
- Use consistent Pixels Per Unit.
- Consider the 2D Pixel Perfect package/component.
- Match reference resolution and art PPU.
- Avoid fractional camera motion if crisp pixel alignment is required.
- Test target resolutions and aspect ratios.

A follow script does not constrain the camera to level bounds. Use Cinemachine confiners or calculate bounds using camera half-width and half-height. Use a separate rig/offset for shake so shake and follow do not fight.

Common mistakes: missing target, wrong Z convention, confusing Size with full view height, excessive smoothing, and sub-pixel blur.
