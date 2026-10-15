# 2D: UI

The guides here use Unity's built-in uGUI system unless stated otherwise. uGUI uses Canvas, RectTransform, Graphic, EventSystem, and UI components.

## Guides
- [Health bar and HUD](./Health-Bar-and-HUD.md)
- [Buttons, menus, and pause](./Buttons-Menus-and-Pause.md)

## Layout basics
Use Canvas Scaler with Scale With Screen Size for scalable screen UI. Use RectTransform anchors and layout groups rather than fixing all positions to absolute pixels. Import TextMeshPro essentials if prompted.

## Input troubleshooting
Verify an active EventSystem and the input module that matches the chosen input backend. Buttons may be blocked by overlapping raycast targets, disabled CanvasGroup settings, or missing On Click listeners. A world-space Canvas has different scale and rendering concerns from a screen-space Canvas.

## Accessibility and test
Test multiple aspect ratios, support readable contrast and meaningful labels, and do not rely on color alone to communicate state.
