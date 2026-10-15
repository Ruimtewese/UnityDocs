# 3D: UI

## Canvas modes
- **Screen Space - Overlay:** UI draws over the scene.
- **Screen Space - Camera:** UI is rendered by a chosen camera.
- **World Space:** Canvas is placed in the scene and behaves like a world object.

Use anchors and layout groups for responsive menus. World-space UI needs deliberate scale, facing, sorting, and camera-distance choices.

## Useful components
Canvas, CanvasScaler, GraphicRaycaster, EventSystem, Button, Image, TextMeshPro, Slider, Toggle, CanvasGroup, and layout components.

## World interaction
Use raycasts or explicit interaction interfaces for selecting objects. Keep UI pointer handling distinct from gameplay rays. UI should display game state rather than become the authority for it.

## Debugging
Check the active EventSystem/input module, raycast targets, Canvas mode, sorting, and whether another element blocks the pointer.

Reference: [Unity UI manual](https://docs.unity3d.com/Manual/UISystem.html)
