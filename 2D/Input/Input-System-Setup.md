# Unity Input System: 2D Setup

Requirements: Unity 6 and the Input System package. Install it through Package Manager. If prompted, enable the new input backend and restart Unity.

## Create actions
1. Create Assets/Input/Gameplay.inputactions and a map named Gameplay.
2. Add Move as Value / Vector2. Bind a 2D Vector composite for WASD and arrows, plus gamepad left stick.
3. Add Jump as Button, bound to Space and a gamepad south button.
4. Add PlayerInput to the player, assign the asset, set Default Map to Gameplay, and choose Send Messages.

## Input reader
Save as PlayerInputReader2D.cs and attach beside PlayerInput.

    using UnityEngine;
    using UnityEngine.InputSystem;
    [RequireComponent(typeof(PlayerInput))]
    public sealed class PlayerInputReader2D : MonoBehaviour
    {
        public Vector2 Move { get; private set; }
        public bool JumpPressed { get; private set; }
        public void OnMove(InputValue value) => Move = value.Get<Vector2>();
        public void OnJump(InputValue value)
        {
            if (value.isPressed) JumpPressed = true;
        }
        public bool ConsumeJumpPressed()
        {
            bool result = JumpPressed;
            JumpPressed = false;
            return result;
        }
        private void OnDisable()
        {
            Move = Vector2.zero;
            JumpPressed = false;
        }
    }

With Send Messages, action names determine callback names: Move calls OnMove and Jump calls OnJump. A physics controller can read Move and consume JumpPressed in its physics logic.

## Common mistakes
- The action map is disabled or Default Map is wrong.
- Callback names do not match action names.
- The action has the wrong value type or missing bindings.
- A one-frame press is lost between Update and FixedUpdate.
- Legacy Input.GetAxis code is used with only the new backend enabled.

Official guide: https://docs.unity3d.com/Packages/com.unity.inputsystem@latest
