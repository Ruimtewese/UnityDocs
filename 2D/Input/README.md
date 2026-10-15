# 2D: Input

## Guides
- [Input System setup](./Input-System-Setup.md)
- [Touch and mobile input](./Touch-and-Mobile-Input.md)

## Input System workflow
Install the Input System package, create an Input Actions asset/action map, define expected value types, assign it to PlayerInput, and select a notification behavior. Send Messages uses callback names matching the actions; Unity Events uses explicit Inspector wiring.

## Design rules
Keep input collection separate from movement/gameplay. A keyboard/gamepad Vector2 or virtual joystick should produce the same intent value where possible. Capture one-shot presses such as jump, then consume them deliberately in the physics step.

## Troubleshooting
Check the active input backend, default action map, bindings, callback names/signatures, UI input module, and focus. Verify mobile touch controls with realistic device aspect ratios and touch interactions.

Official docs: https://docs.unity3d.com/Packages/com.unity.inputsystem@latest
