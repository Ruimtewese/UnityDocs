# 3D CharacterController — Complete Setup

Use CharacterController for a script-driven character that needs collision-constrained movement without normal Dynamic Rigidbody simulation. Gravity and jump behaviour must be implemented by your controller.

## Scene setup
1. Create a Capsule GameObject or character model.
2. Add a **CharacterController** and size its Height, Radius, Center, and Step Offset to fit the character.
3. Add a child Camera or camera rig; do not place the camera at ground level.
4. Create an Input Actions asset with Move (Value / Vector2) and Jump (Button).
5. Assign actions in the Inspector.
6. Confirm the controller's capsule does not start intersecting the floor.

## Script: SimpleCharacterController3D.cs

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

[RequireComponent(typeof(CharacterController))]
public sealed class SimpleCharacterController3D : MonoBehaviour
{
    [Header("References")]
    [SerializeField] private Transform cameraTransform;
    [SerializeField] private InputActionReference moveAction;
    [SerializeField] private InputActionReference jumpAction;

    [Header("Movement")]
    [SerializeField, Min(0f)] private float moveSpeed = 5f;
    [SerializeField, Min(0f)] private float jumpHeight = 1.5f;
    [SerializeField] private float gravity = -20f;

    private CharacterController controller;
    private Vector3 verticalVelocity;
    private Vector2 moveInput;
    private bool jumpRequested;

    private void Awake() => controller = GetComponent<CharacterController>();

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
        moveInput = moveAction != null
            ? Vector2.ClampMagnitude(moveAction.action.ReadValue<Vector2>(), 1f)
            : Vector2.zero;

        Vector3 forward = cameraTransform != null
            ? cameraTransform.forward : transform.forward;
        Vector3 right = cameraTransform != null
            ? cameraTransform.right : transform.right;

        forward.y = 0f;
        right.y = 0f;
        forward.Normalize();
        right.Normalize();

        Vector3 horizontal = (right * moveInput.x + forward * moveInput.y) * moveSpeed;

        if (controller.isGrounded && verticalVelocity.y < 0f)
            verticalVelocity.y = -2f;

        if (controller.isGrounded && jumpRequested)
        {
            verticalVelocity.y = Mathf.Sqrt(jumpHeight * -2f * gravity);
            jumpRequested = false;
        }
        else if (!controller.isGrounded)
        {
            jumpRequested = false;
        }

        verticalVelocity.y += gravity * Time.deltaTime;
        controller.Move((horizontal + verticalVelocity) * Time.deltaTime);
    }

    private void OnJump(InputAction.CallbackContext context) => jumpRequested = true;
}
```

## Important details
- `CharacterController.Move` receives a **displacement**, not a velocity, so multiply velocity by `Time.deltaTime`.
- Move does not apply gravity automatically.
- The jump equation `v = sqrt(height * -2 * gravity)` assumes gravity is negative and height is positive.
- Camera-relative directions are flattened onto the ground plane to prevent camera pitch from steering the player into the ground.
- The small negative grounded velocity helps maintain contact on slopes; tune it to your project.

## Inspector settings
- CharacterController Center should place the capsule around the character's body.
- Height and Radius should match the model, not its weapon or accessories.
- Slope Limit and Step Offset should suit level geometry.
- Move and Jump actions must reference the intended actions.

## Troubleshooting
- **No jump:** confirm the Jump action and check whether the controller reports grounded.
- **Moves diagonally faster:** clamp or normalize the input vector.
- **Moves toward sky/ground when camera tilts:** flatten camera forward/right vectors.
- **Falls through floor:** check collider setup, scene geometry, and controller placement.
- **Bumps on small steps:** inspect Step Offset and actual step dimensions.

Reference: [CharacterController API](https://docs.unity3d.com/ScriptReference/CharacterController.html)
