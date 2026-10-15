# Unity Input System: 3D Setup

Requirements: Unity Input System package. Create an Input Actions asset and Gameplay map. Add Move (Value / Vector2) bound to WASD/arrows and gamepad left stick; Look (Value / Vector2) bound to mouse delta and gamepad right stick; Jump (Button) bound to Space and gamepad south button. Add PlayerInput, assign the asset, set Default Map to Gameplay, and choose Send Messages.

## Reader component
Save as InputReader3D.cs and attach beside PlayerInput.

    using UnityEngine;
    using UnityEngine.InputSystem;
    [RequireComponent(typeof(PlayerInput))]
    public sealed class InputReader3D : MonoBehaviour
    {
        public Vector2 Move { get; private set; }
        public Vector2 Look { get; private set; }
        public bool JumpPressed { get; private set; }
        public void OnMove(InputValue value) => Move = value.Get<Vector2>();
        public void OnLook(InputValue value) => Look = value.Get<Vector2>();
        public void OnJump(InputValue value)
        {
            if (value.isPressed) JumpPressed = true;
        }
        public bool ConsumeJump()
        {
            bool result = JumpPressed;
            JumpPressed = false;
            return result;
        }
        private void OnDisable()
        {
            Move = Vector2.zero;
            Look = Vector2.zero;
            JumpPressed = false;
        }
    }

Send Messages expects OnMove, OnLook, and OnJump for actions named Move, Look, and Jump. The controller converts Move to camera-relative or player-relative world movement.

Mouse delta is per-frame displacement; gamepad stick input is a rate. Sensitivity and time scaling often differ. For rebinding and multi-device support, use action assets instead of hardcoded keys.

Official guide: https://docs.unity3d.com/Packages/com.unity.inputsystem@latest
