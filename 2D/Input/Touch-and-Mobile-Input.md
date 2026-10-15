# 2D Touch and Mobile Input

This guide adds a basic on-screen joystick for mobile builds. It uses uGUI and Unity's EventSystem; create a Canvas, an EventSystem, a background Image, and a smaller handle Image. Put the handle under the background and assign both RectTransforms.

## Virtual joystick
Save as VirtualJoystick.cs and attach to the background Image. Its Image must receive raycasts.

    using UnityEngine;
    using UnityEngine.EventSystems;

    public sealed class VirtualJoystick : MonoBehaviour,
        IPointerDownHandler, IDragHandler, IPointerUpHandler
    {
        [SerializeField] private RectTransform background;
        [SerializeField] private RectTransform handle;
        [SerializeField, Min(1f)] private float handleRange = 60f;
        public Vector2 Value { get; private set; }

        public void OnPointerDown(PointerEventData eventData) => OnDrag(eventData);

        public void OnDrag(PointerEventData eventData)
        {
            if (background == null || handle == null) return;
            if (!RectTransformUtility.ScreenPointToLocalPointInRectangle(
                background, eventData.position, eventData.pressEventCamera, out Vector2 local))
                return;

            Vector2 radius = background.rect.size * 0.5f;
            if (radius.x <= 0f || radius.y <= 0f) return;
            Value = Vector2.ClampMagnitude(
                new Vector2(local.x / radius.x, local.y / radius.y), 1f);
            handle.anchoredPosition = new Vector2(Value.x * handleRange, Value.y * handleRange);
        }

        public void OnPointerUp(PointerEventData eventData)
        {
            Value = Vector2.zero;
            if (handle != null) handle.anchoredPosition = Vector2.zero;
        }
    }

Adjust handleRange to match the joystick's RectTransform units and inspect anchors/pivot. The component reports Value; a movement controller must read it and apply movement.

## Input backend and testing
- Use the Input System's UI Input Module if the project uses the new Input System.
- Test with actual screen aspect ratios and safe areas; fixed pixel positions can be clipped by notches/rounded corners.
- UI images need Raycast Target enabled to receive pointer events.
- Keep touch buttons large enough to tap and don't rely only on color for state.
- A virtual joystick is a UI control, not a full mobile control scheme; add action buttons for jump, interact, or fire.

Official Input System docs: https://docs.unity3d.com/Packages/com.unity.inputsystem@latest
