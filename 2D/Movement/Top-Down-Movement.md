# 2D Top-Down Movement

This example moves a character in four or eight directions with a Rigidbody2D. It uses the Input System package and keeps diagonal speed consistent.

## Setup
- Player: SpriteRenderer, Rigidbody2D (Dynamic), and an appropriate Collider2D.
- Freeze Rigidbody2D rotation Z if the sprite should remain upright.
- Create a Move action (Value / Vector2), bind WASD and arrow keys, and assign it to the script.
- Set Linear Drag/Damping according to the feel you want; damping can make the character coast.

## Script: TopDownMovement2D.cs

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

[RequireComponent(typeof(Rigidbody2D))]
public sealed class TopDownMovement2D : MonoBehaviour
{
    [SerializeField, Min(0f)] private float speed = 5f;
    [SerializeField] private InputActionReference moveAction;

    private Rigidbody2D body;
    private Vector2 moveInput;

    private void Awake() => body = GetComponent<Rigidbody2D>();

    private void OnEnable()
    {
        if (moveAction != null) moveAction.action.Enable();
    }

    private void OnDisable()
    {
        if (moveAction != null) moveAction.action.Disable();
    }

    private void Update()
    {
        moveInput = moveAction != null
            ? Vector2.ClampMagnitude(moveAction.action.ReadValue<Vector2>(), 1f)
            : Vector2.zero;
    }

    private void FixedUpdate()
    {
        body.linearVelocity = moveInput * speed;
    }
}
```

## Why ClampMagnitude?
Without normalization/clamping, simultaneous horizontal and vertical input can have magnitude √2, making diagonal movement faster. ClampMagnitude keeps input magnitude at or below one.

## Alternative movement styles
- Set Rigidbody2D velocity for direct, responsive arcade movement.
- Use AddForce for acceleration-based movement; tune mass, force, and damping together.
- Use MovePosition for an intentionally kinematic controller, after checking its collision behaviour.
- Use Transform movement only when physics response is not required.

## Troubleshooting
If the player slides, reduce damping or stop velocity when input is zero. If the player passes through thin walls at high speed, inspect collision detection and fixed timestep settings. Do not apply both velocity control and force control without a clear reason.
