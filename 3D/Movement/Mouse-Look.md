# First-person mouse look

This guide uses the legacy Input Manager. It assumes a camera is a child of a player root, and the player root rotates horizontally while the camera pivots vertically.

## Setup

1. Create a Player root and child Camera.
2. Attach this script to the Player root.
3. Assign the camera transform to Camera Pivot.
4. Set the cursor lock when entering gameplay and release it when opening a menu.

## MouseLook.cs

```csharp
using UnityEngine;

public sealed class MouseLook : MonoBehaviour
{
    [SerializeField] private Transform cameraPivot;
    [SerializeField, Min(0f)] private float sensitivity = 2f;
    [SerializeField] private float minPitch = -80f;
    [SerializeField] private float maxPitch = 80f;

    private float pitch;

    private void Start()
    {
        Cursor.lockState = CursorLockMode.Locked;
        Cursor.visible = false;
    }

    private void Update()
    {
        if (cameraPivot == null) return;

        float mouseX = Input.GetAxis("Mouse X") * sensitivity;
        float mouseY = Input.GetAxis("Mouse Y") * sensitivity;

        transform.Rotate(Vector3.up * mouseX);

        pitch = Mathf.Clamp(pitch - mouseY, minPitch, maxPitch);
        cameraPivot.localRotation = Quaternion.Euler(pitch, 0f, 0f);

        if (Input.GetKeyDown(KeyCode.Escape))
        {
            Cursor.lockState = CursorLockMode.None;
            Cursor.visible = true;
        }
    }
}
```

For a production game, handle cursor relocking through an explicit pause/menu state and use Input System actions when the project uses the newer input package. Avoid multiple scripts rotating the same transforms.
