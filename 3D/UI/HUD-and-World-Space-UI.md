# 3D UI: HUD and World-Space Labels

## Canvas modes
- Screen Space - Overlay: HUD over the camera.
- Screen Space - Camera: UI rendered relative to a camera.
- World Space: UI is a plane in the 3D scene, useful for nameplates and world health bars.

## World-space setup
Create a Canvas, set Render Mode to World Space, size its RectTransform, and reduce its scale. A scale such as 0.01 may be a starting point but is not universal. Add TextMeshPro text and make it face the intended camera.

## Billboard script
Save as BillboardToCamera.cs and attach to the world-space UI root.

    using UnityEngine;
    public sealed class BillboardToCamera : MonoBehaviour
    {
        [SerializeField] private Camera targetCamera;
        private void LateUpdate()
        {
            if (targetCamera == null) targetCamera = Camera.main;
            if (targetCamera == null) return;
            transform.rotation = targetCamera.transform.rotation;
        }
    }

For labels that must remain upright, use only the camera's horizontal forward direction instead of copying pitch and roll.

## Rules and troubleshooting
Use anchors and Canvas Scaler for screen HUDs. Ensure EventSystem uses the correct input module. World-space interactive UI may need a Graphic Raycaster. Configure sorting/order so labels are visible. If buttons do not respond, inspect EventSystem, input module, Graphic Raycaster, Raycast Target, CanvasGroup, and overlapping elements.
