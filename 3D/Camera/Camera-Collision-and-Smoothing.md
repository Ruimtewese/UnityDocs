# 3D Third-Person Camera Collision and Smoothing

A third-person camera should not clip through walls. Choose a target pivot near the character and a desired offset behind it. Cast from the pivot toward that offset, excluding the player layer when appropriate.

## Collision distance example
Attach to a camera rig. Assign targetPivot, and set the local desired camera offset. The script moves the rig's child camera along the vector between pivot and desired position.

    using UnityEngine;

    public sealed class ThirdPersonCameraCollision : MonoBehaviour
    {
        [SerializeField] private Transform targetPivot;
        [SerializeField] private Transform cameraTransform;
        [SerializeField] private Vector3 localOffset = new Vector3(0f, 1f, -4f);
        [SerializeField, Min(0.01f)] private float sphereRadius = 0.2f;
        [SerializeField, Min(0f)] private float padding = 0.1f;
        [SerializeField] private LayerMask obstacleLayers = ~0;

        private void LateUpdate()
        {
            if (targetPivot == null || cameraTransform == null) return;

            Vector3 desired = targetPivot.TransformPoint(localOffset);
            Vector3 direction = desired - targetPivot.position;
            float distance = direction.magnitude;
            if (distance < 0.001f) return;
            direction /= distance;

            float allowedDistance = distance;
            if (Physics.SphereCast(targetPivot.position, sphereRadius, direction,
                    out RaycastHit hit, distance, obstacleLayers,
                    QueryTriggerInteraction.Ignore))
                allowedDistance = Mathf.Max(0.05f, hit.distance - padding);

            cameraTransform.position = targetPivot.position + direction * allowedDistance;
            cameraTransform.rotation = Quaternion.LookRotation(
                targetPivot.position - cameraTransform.position, Vector3.up);
        }
    }

This is a basic collision solver, not a replacement for Cinemachine's camera collision extension. Ensure obstacleLayers excludes the player and camera target. For smoothing, filter the allowed distance carefully so the camera moves inward quickly when blocked but returns outward without lagging too much.

## Common mistakes
- SphereCast hits the player's own collider.
- Offset uses the wrong local axis for the camera rig.
- Camera becomes too close to the pivot at a wall and clips into the player.
- Camera follow and this collision script write the same position independently.
