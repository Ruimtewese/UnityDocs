# 3D: Input

## Guide
- [Input System setup](./Input-System-Setup.md)

## Setup
Install Input System, create a Gameplay action map, add Move (Vector2), Look (Vector2), and action buttons such as Jump/Interact/Fire. Assign the input asset to PlayerInput and select the default map and callback behavior.

## Separate input from control
The input reader reports intent. The movement controller converts it into world movement; camera logic consumes look input; interaction/weapon logic consumes button presses. This separation makes AI, replay, and rebinding easier.

## Mouse versus stick
Mouse delta is a displacement per frame. A gamepad stick is a direction/rate and often needs deltaTime scaling. Use separate sensitivities and clamp the stick as appropriate.

## Common mistakes
Action map is disabled, callback name mismatch, unassigned Input Actions asset, old and new input APIs conflict, or cursor focus is not restored for menus.

Official docs: https://docs.unity3d.com/Packages/com.unity.inputsystem@latest
