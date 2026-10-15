# 2D: Input

For new projects that need rebinding, controller support, multiple devices, or UI integration, consider the Input System package. Legacy Input Manager examples may still be useful for older projects.

## Suggested actions
- Move — Value / Vector2
- Jump — Button
- Interact — Button
- Attack — Button
- Pause — Button

## Implementation practices
- Keep input collection separate from movement logic.
- Use action callbacks or read action values in a controlled update loop.
- Enable and disable action maps with the owning gameplay/menu state.
- Convert pointer positions to world coordinates through the active camera when interacting with world objects.
- Normalize movement input if equal diagonal speed is desired.

## Common mistakes
Screen coordinates are not world units. A disabled object should not leave stale callbacks subscribed. Scattered raw key checks make rebinding and gamepad support harder.

Reference: [Input System manual](https://docs.unity3d.com/Packages/com.unity.inputsystem@latest)
