# 3D: Input

The Input System package is useful for action maps, rebinding, controller support, and UI integration. Legacy input can still be relevant to existing projects.

## Suggested actions
- Move — Vector2
- Look — Vector2
- Jump, Sprint, Interact, Pause — Buttons
- Fire/Primary Action — Button, if applicable to the game

## Good architecture
Keep input reading separate from movement and camera logic. Use separate action maps for gameplay and menus when useful. Mouse delta and gamepad stick input have different sensitivity and smoothing needs.

For aiming or selecting world objects, create a ray from the active camera and use an explicit layer mask. For pointer UI, use the configured UI input module instead of treating every click as a world ray.

## Common mistakes
Screen coordinates are not world positions. Disabled controllers should not leave callbacks subscribed. Centralized actions are easier to rebind than scattered raw key checks.

Reference: [Input System manual](https://docs.unity3d.com/Packages/com.unity.inputsystem@latest)
