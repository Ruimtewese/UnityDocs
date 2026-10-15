# 2D Parallax Background

Parallax creates perceived depth by moving background layers at a fraction of camera motion. Place far layers behind nearer layers and assign each a different factor.

## Parallax component
Save as ParallaxLayer2D.cs. Attach to a background layer Transform and assign the camera. Use small factors for distant layers; factors above 1 create faster foreground motion.

    using UnityEngine;

    public sealed class ParallaxLayer2D : MonoBehaviour
    {
        [SerializeField] private Transform cameraTransform;
        [SerializeField, Range(-1f, 1f)] private float factor = 0.3f;
        private Vector3 previousCameraPosition;

        private void Start()
        {
            if (cameraTransform == null && Camera.main != null)
                cameraTransform = Camera.main.transform;
            if (cameraTransform != null)
                previousCameraPosition = cameraTransform.position;
        }

        private void LateUpdate()
        {
            if (cameraTransform == null) return;
            Vector3 delta = cameraTransform.position - previousCameraPosition;
            transform.position += new Vector3(delta.x * factor, delta.y * factor, 0f);
            previousCameraPosition = cameraTransform.position;
        }
    }

## Tiling
For an infinitely repeating background, use a tiled sprite/material or reposition segments when they pass the camera bounds. Calculate segment width in world units (sprite pixels divided by Pixels Per Unit) and account for camera view width. Avoid moving the same background from multiple scripts.

## Common mistakes
- Camera reference is missing or Camera.main cannot find a tagged MainCamera.
- Background is moved by both parallax and another script.
- Factor is applied to absolute camera position each frame instead of camera delta, causing drift.
- Sprite seams appear due to non-tileable art, filtering, or fractional scaling.
- Background covers gameplay because its Z/sorting order is wrong.
