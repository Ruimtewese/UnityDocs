# 3D: Input

## Guides
- [Input System setup](./Input-System-Setup.md)
- [Cursor lock and mouse sensitivity](./Cursor-Lock-and-Mouse-Sensitivity.md)
- [Runtime input rebinding](./Runtime-Input-Rebinding.md)

## Setup
Install Input System, create a Gameplay action map, add Move (Vector2), Look (Vector2), and buttons such as Jump/Interact, assign the Input Actions asset to PlayerInput, and select the default map and callback behavior.

## Separate input from control
The input reader reports intent. The movement controller converts it into world movement; camera logic consumes look input; interaction systems consume button presses. This separation makes AI, replay, and rebinding easier.

## Mouse versus stick
Mouse delta is displacement per frame. A gamepad stick is a rate and often needs deltaTime scaling. Use separate sensitivities. When rebinding, target the correct binding index (not a composite root), dispose the rebind operation, and save overrides if they should persist.

Official docs: https://docs.unity3d.com/Packages/com.unity.inputsystem@latest
