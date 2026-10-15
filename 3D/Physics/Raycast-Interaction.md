# 3D Raycast Interaction

A raycast tests for a collider along a line. It is commonly used for looking at objects, hitscan tools, ground checks, and selecting world objects.

## Scene setup
- Put interactable objects on an Interactable layer.
- Give each object a collider.
- Create a LayerMask containing Interactable.
- Add this script to the player or camera rig and assign the camera.

## Example: inspect target

```csharp
using UnityEngine;

public sealed class RaycastInteractor : MonoBehaviour
{
    [SerializeField] private Camera viewCamera;
    [SerializeField, Min(0.1f)] private float distance = 3f;
    [SerializeField] private LayerMask interactableLayers;

    private void Update()
    {
        if (viewCamera == null) return;

        Ray ray = viewCamera.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0f));

        if (Physics.Raycast(ray, out RaycastHit hit, distance, interactableLayers))
        {
            Debug.DrawLine(ray.origin, hit.point, Color.green);

            if (Keyboard.current != null &&
                Keyboard.current.eKey.wasPressedThisFrame)
            {
                Debug.Log($"Looked at: {hit.collider.name}", hit.collider);
                // Call an interaction interface here.
            }
        }
        else
        {
            Debug.DrawRay(ray.origin, ray.direction * distance, Color.gray);
        }
    }
}
```

This example uses the Input System keyboard API, so include `using UnityEngine.InputSystem;` and ensure the Input System package is installed. For a production project, separate input from raycast detection and implement a shared interface such as `IInteractable`.

## Why use a LayerMask?
Without filtering, the ray may hit the player, invisible triggers, decoration, or unrelated colliders. A dedicated layer makes target selection predictable.

## Debugging
- Confirm the camera reference and active camera.
- Confirm the target has an enabled collider.
- Check the target's Layer and the LayerMask.
- Verify the ray's origin and direction in the Scene view.
- Ensure another collider is not blocking the target.
- Check whether the chosen distance is long enough.

Reference: [Physics.Raycast](https://docs.unity3d.com/ScriptReference/Physics.Raycast.html)
