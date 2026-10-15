# 3D Camera: First- and Third-Person Foundations

For first person, parent the camera to a pivot at eye height. For third person, follow a pivot near shoulder height. Separate character yaw from camera pitch: yaw rotates the player around world Y, while pitch rotates a child pivot around local X.

## Mouse look
Save as MouseLook3D.cs. Attach to the player root and assign a **separate child Transform** as pitchPivot. This script requires the Input System package.

    using UnityEngine;
    using UnityEngine.InputSystem;

    public sealed class MouseLook3D : MonoBehaviour
    {
        [SerializeField] private Transform pitchPivot;
        [SerializeField, Min(0f)] private float sensitivity = 0.08f;
        [SerializeField] private float minPitch = -80f;
        [SerializeField] private float maxPitch = 80f;
        private float pitch;

        private void Start()
        {
            if (pitchPivot == null || pitchPivot == transform)
            {
                Debug.LogError("Assign a separate child camera pitch pivot.", this);
                enabled = false;
                return;
            }

            pitch = pitchPivot.localEulerAngles.x;
            if (pitch > 180f) pitch -= 360f;
            LockCursor(true);
        }

        private void Update()
        {
            if (Mouse.current != null && Cursor.lockState == CursorLockMode.Locked)
            {
                Vector2 delta = Mouse.current.delta.ReadValue() * sensitivity;
                transform.Rotate(Vector3.up, delta.x, Space.World);
                pitch = Mathf.Clamp(pitch - delta.y, minPitch, maxPitch);
                pitchPivot.localRotation = Quaternion.Euler(pitch, 0f, 0f);
            }

            if (Keyboard.current != null && Keyboard.current.escapeKey.wasPressedThisFrame)
                LockCursor(false);
            if (Mouse.current != null && Mouse.current.leftButton.wasPressedThisFrame)
                LockCursor(true);
        }

        private static void LockCursor(bool locked)
        {
            Cursor.lockState = locked ? CursorLockMode.Locked : CursorLockMode.None;
            Cursor.visible = !locked;
        }

        private void OnDisable() => LockCursor(false);
    }

The root handles yaw; the child pivot handles pitch. The camera should be a child of pitchPivot or attached beneath it. This is mouse-look only, not a full character controller. Gamepad look needs stick handling. Integrate cursor behavior with pause/settings menus rather than allowing two systems to fight over cursor state.

## Third-person cameras
A third-person camera needs wall collision. Use Cinemachine follow/collider features when available, or cast from the target pivot toward the desired camera position and shorten the distance to obstacles.

## Common mistakes
- pitchPivot is the same Transform as the yaw root.
- A camera and player rotate on all axes together.
- Cursor remains locked while a menu is open.
- Pitch is not clamped.
- Camera clips through walls or player geometry.
