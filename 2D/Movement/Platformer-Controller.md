# 2D Platformer Controller — Complete Setup

This example is a starting point for a simple platformer using the **Input System** package and a **Dynamic Rigidbody2D**. It is intentionally small; add coyote time, jump buffering, slopes, ladders, and moving-platform support as separate features.

## Scene setup

1. Create a Player GameObject with a SpriteRenderer.
2. Add **Rigidbody2D**: Body Type = Dynamic; Freeze Rotation Z = enabled; Collision Detection = Continuous if fast motion warrants it; Interpolate = Interpolate if movement looks jittery.
3. Add a **CapsuleCollider2D** or **BoxCollider2D**, sized to the character.
4. Create a Ground layer and assign it to level colliders.
5. Create a child empty object named `GroundCheck`, positioned just below the feet.
6. Install/enable the Input System package. Create an Input Actions asset with a Gameplay map: Move (Value / Vector2) bound to WASD and arrow keys, and Jump (Button) bound to Space.
7. Assign the asset and actions in the Inspector.

## Script: PlatformerController2D.cs

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

[RequireComponent(typeof(Rigidbody2D))]
public sealed class PlatformerController2D : MonoBehaviour
{
    [Header("Movement")]
    [SerializeField, Min(0f)] private float moveSpeed = 7f;
    [SerializeField, Min(0f)] private float jumpVelocity = 13f;

    [Header("Ground check")]
    [SerializeField] private Transform groundCheck;
    [SerializeField, Min(0.01f)] private float groundRadius = 0.12f;
    [SerializeField] private LayerMask groundLayers;

    [Header("Input actions")]
    [SerializeField] private InputActionReference moveAction;
    [SerializeField] private InputActionReference jumpAction;

    private Rigidbody2D body;
    private bool grounded;
    private bool jumpQueued;
    private float horizontal;

    private void Awake()
    {
        body = GetComponent<Rigidbody2D>();
    }

    private void OnEnable()
    {
        if (moveAction != null) moveAction.action.Enable();
        if (jumpAction != null)
        {
            jumpAction.action.Enable();
            jumpAction.action.performed += OnJump;
        }
    }

    private void OnDisable()
    {
        if (jumpAction != null)
        {
            jumpAction.action.performed -= OnJump;
            jumpAction.action.Disable();
        }

        if (moveAction != null) moveAction.action.Disable();
    }

    private void Update()
    {
        horizontal = moveAction != null
            ? moveAction.action.ReadValue<Vector2>().x
            : 0f;

        grounded = groundCheck != null &&
            Physics2D.OverlapCircle(groundCheck.position, groundRadius, groundLayers);

    }

    private void FixedUpdate()
    {
        Vector2 velocity = body.linearVelocity;
        velocity.x = horizontal * moveSpeed;

        if (grounded && jumpQueued)
        {
            velocity.y = jumpVelocity;
            jumpQueued = false;
        }

        body.linearVelocity = velocity;
    }

    private void OnJump(InputAction.CallbackContext context)
    {
        jumpQueued = true;
    }

    private void OnDrawGizmosSelected()
    {
        if (groundCheck == null) return;
        Gizmos.color = Color.yellow;
        Gizmos.DrawWireSphere(groundCheck.position, groundRadius);
    }
}
```

> API note: recent Unity versions expose Rigidbody2D's linearVelocity through the documented Rigidbody2D API; check the API for the exact Unity version in your project if a member name differs.

## Inspector checklist
- Move Action points to the Move action, not the whole action map.
- Jump Action points to Jump.
- Ground Check is below the feet, not inside the body.
- Ground Layers includes only surfaces that should count as ground.
- Rigidbody2D rotation is frozen so the character does not tip over.
- The Input System package and active input handling are configured for the project.

## Why the code is structured this way
- `Update` reads input and checks the ground state.
- `FixedUpdate` applies horizontal physics movement.
- Jump is queued so a button press can be consumed by the physics loop.
- `OnDisable` unsubscribes callbacks to avoid duplicate subscriptions.
- The Gizmo makes the ground probe visible in the Scene view.

## Common problems
- **Never grounded:** move GroundCheck lower, enlarge its radius slightly, verify Ground Layers and the Physics 2D collision setup.
- **Jump does nothing:** inspect the Jump binding and Console; ensure the action is enabled.
- **Character rotates:** freeze Rigidbody2D rotation Z.
- **Jitter:** avoid also moving the Transform directly; try Rigidbody2D interpolation.
- **Old Unity API mismatch:** consult the version-specific [Rigidbody2D API](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html).

## Next improvements
Add jump buffering (remember a recent jump press), coyote time (allow a short grace period after leaving a ledge), variable jump height, acceleration/deceleration, and one-way platforms. Implement each feature independently and test edge cases.
