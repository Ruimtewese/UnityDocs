# 3D: UI

## Guides
- [HUD and world-space UI](./HUD-and-World-Space-UI.md)
- [Settings menu and audio volume](./Settings-Menu-and-Audio-Volume.md)
- [UI Toolkit versus uGUI](./UI-Toolkit-vs-uGUI.md)

## Canvas modes
- Screen Space - Overlay: HUD over the camera.
- Screen Space - Camera: UI rendered relative to a camera.
- World Space: UI positioned in the 3D world, such as nameplates or health bars.

## Layout and input
Use anchors, Canvas Scaler, and layout groups to support different resolutions. Ensure EventSystem uses a compatible input module. World-space interaction may need Graphic Raycaster. Raycast targets and CanvasGroup settings can block input.

## Settings
Use an AudioMixer for grouped volume controls, expose a mixer parameter, and convert a linear slider value to decibels. Persist user settings separately from security-sensitive data.

## UI Toolkit
UI Toolkit uses UXML/USS and VisualElement rather than uGUI's Canvas/RectTransform pattern. Choose one system intentionally for each interface and confirm runtime support in the installed Unity version.
