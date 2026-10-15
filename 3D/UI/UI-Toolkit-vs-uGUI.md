# Unity UI Toolkit vs uGUI: Choosing a UI System

Unity has more than one UI workflow. Choose based on the project and team; do not mix systems accidentally on the same screen without a deliberate reason.

## uGUI (Canvas UI)
Often a practical choice for game HUDs, buttons, health bars, world-space labels, and projects already using Canvas/RectTransform.
- Uses Canvas, RectTransform, Graphic components, EventSystem, and UI input modules.
- Layout commonly uses anchors, layout groups, and Canvas Scaler.
- Canvas can be Screen Space - Overlay, Screen Space - Camera, or World Space.

## UI Toolkit
Uses UXML for structure, USS for styling, and C# for behavior. It is common for Editor extensions and also supports runtime UI.
- Visual elements are laid out in a hierarchy rather than using RectTransform for every element.
- Styling is CSS-like but follows UI Toolkit's USS subset and rules.
- Runtime interactions use UI Toolkit events and controls, not uGUI Button OnClick wiring.
- Exact features vary across Unity versions; verify runtime UI support and limitations for the installed version.

## Choosing
Use the system the project already uses unless there is a clear reason to migrate. A small HUD or tutorial following an existing uGUI guide should keep using uGUI. A data-heavy panel or UI designed primarily with UXML/USS may suit UI Toolkit.

## Input and accessibility
Ensure keyboard/gamepad navigation, pointer/touch interaction, focus states, and readable scaling work for the target device. Avoid mixing two EventSystem/input paths without testing.

## Common mistakes
- Following a UI Toolkit event tutorial while the object is a uGUI Canvas.
- Assuming RectTransform exists on UI Toolkit VisualElements.
- Mixing incompatible event systems or input modules.
- Migrating UI without checking platform support, navigation, and layout.
Official manual: https://docs.unity3d.com/Manual/UIElements.html
