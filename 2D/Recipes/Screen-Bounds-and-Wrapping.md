# 2D Recipe: Viewport Bounds and Object Wrapping

## Convert screen edges to world positions
Viewport coordinates are normalized: bottom-left is (0,0), top-right is (1,1). The camera converts them to world coordinates.

    using UnityEngine;

    public sealed class ViewBounds2D : MonoBehaviour
    {
        [SerializeField] private Camera targetCamera;
        public Bounds WorldBounds { get; private set; }

        private void LateUpdate()
        {
            if (targetCamera == null) targetCamera = Camera.main;
            if (targetCamera == null) return;

            Vector3 bottomLeft = targetCamera.ViewportToWorldPoint(
                new Vector3(0f, 0f, Mathf.Abs(targetCamera.transform.position.z)));
            Vector3 topRight = targetCamera.ViewportToWorldPoint(
                new Vector3(1f, 1f, Mathf.Abs(targetCamera.transform.position.z)));

            WorldBounds = new Bounds(
                (bottomLeft + topRight) * 0.5f,
                topRight - bottomLeft);
        }
    }

This depth calculation assumes a conventional 2D orthographic camera at negative Z with world objects near Z = 0. If your scene uses another plane, calculate the correct camera-space depth instead.

## Wrapping objects
For a simple space game, wrap an object after it fully exits the view. Decide if wrapping uses center position or renderer bounds; center-only wrapping is shorter but may pop at the edge. Use a camera viewport test for each object, or calculate world bounds and reposition across the opposite edge.

## Common mistakes
- ViewportToWorldPoint z is camera-relative depth, not the world Z coordinate.
- Perspective camera depth calculations differ from orthographic 2D conventions.
- The player's collider or sprite appears clipped when wrapping only the center.
- Multiple camera setups or split-screen mean Camera.main is not always the correct camera.
