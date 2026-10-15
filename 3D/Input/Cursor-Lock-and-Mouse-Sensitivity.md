# 3D Mouse Cursor Lock and Sensitivity

Cursor lock is useful for first-person or third-person camera control. Menus should generally unlock the cursor so buttons can be clicked. Decide how window focus and pause state should restore it.

## Cursor controller
Save as CursorLockController.cs. Connect LockCursor to the gameplay-start button and UnlockCursor to pause/menu buttons.

    using UnityEngine;
    using UnityEngine.InputSystem;

    public sealed class CursorLockController : MonoBehaviour
    {
        [SerializeField] private bool lockOnStart = true;

        private void Start()
        {
            SetLocked(lockOnStart);
        }

        public void LockCursor() => SetLocked(true);
        public void UnlockCursor() => SetLocked(false);

        private static void SetLocked(bool locked)
        {
            Cursor.lockState = locked ? CursorLockMode.Locked : CursorLockMode.None;
            Cursor.visible = !locked;
        }

        private void OnApplicationFocus(bool hasFocus)
        {
            if (!hasFocus) SetLocked(false);
        }

        private void OnDisable() => SetLocked(false);
    }

If you keep gameplay input in a separate script, don't let both scripts independently change Cursor.lockState. Re-locking should be an intentional action after returning to gameplay; don't automatically trap the cursor as soon as focus returns if a menu is open.

## Mouse sensitivity
Mouse delta is already a per-frame displacement in the Input System. Apply sensitivity to that delta, and do not blindly multiply mouse delta by Time.deltaTime. A gamepad stick is a rate input and usually needs Time.deltaTime. Use different sensitivities for mouse and stick.

## Common mistakes
- Cursor remains locked over a settings menu.
- Cursor locks again on application focus when the player is still in a menu.
- Sensitivity is scaled twice.
- Camera input continues while paused or UI is active.
