# 2D: UI

## Canvas modes
- **Screen Space - Overlay:** UI renders over the scene without a camera reference.
- **Screen Space - Camera:** UI is rendered using a selected camera.
- **World Space:** UI exists as a scene object in world coordinates.

Use anchors and layout components to support different resolutions. Buttons should call a focused controller or UI method; avoid placing all game logic inside UI callbacks.

## Useful components
Canvas, CanvasScaler, GraphicRaycaster, EventSystem, Button, Image, TextMeshPro text, Slider, Toggle, CanvasGroup, LayoutGroup, and ContentSizeFitter.

## Troubleshooting
If clicks fail, check EventSystem/Input System UI module, raycast targets, canvas mode, and whether another UI element intercepts the pointer. Keep UI as a display/control layer rather than the source of truth for gameplay state.

Reference: [Unity UI manual](https://docs.unity3d.com/Manual/UISystem.html)
