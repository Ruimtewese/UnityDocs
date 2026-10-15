# 3D Rigidbody Movement Patterns

Use a Rigidbody controller when collision response, forces, momentum, or other physics interactions are important. Pick one movement model and keep it consistent.

## Recommended setup
- Rigidbody: Is Kinematic disabled for a physics-driven player; configure gravity, interpolation, and collision detection for the game.
- CapsuleCollider: sized to the character.
- Freeze unwanted rotation axes if the player must remain upright.
- Input actions: Move (Vector2), with optional Jump (Button).

## Velocity-driven movement
For a simple arcade controller, read input in Update and apply the intended horizontal velocity in FixedUpdate. Keep vertical velocity from gravity/jumps intact. This example uses the Unity 6 Rigidbody.linearVelocity API. For older Editor versions, check the version-specific API documentation.

```csharp
using UnityEngine;

[RequireComponent(typeof(Rigidbody))]
public sealed class RigidbodyPlanarMover : MonoBehaviour
{
    [SerializeField, Min(0f)] private float speed = 5f;
    private Rigidbody body;
    private Vector2 input;

    private void Awake() => body = GetComponent<Rigidbody>();

    public void SetMoveInput(Vector2 value)
    {
        input = Vector2.ClampMagnitude(value, 1f);
    }

    private void FixedUpdate()
    {
        Vector3 horizontal = new Vector3(input.x, 0f, input.y) * speed;
        Vector3 velocity = body.linearVelocity;
        velocity.x = horizontal.x;
        velocity.z = horizontal.z;
        body.linearVelocity = velocity;
    }
}
```

This sample intentionally exposes `SetMoveInput` rather than binding a particular input package. Call it from your chosen input layer. For older Editor versions, adapt to the version-specific Rigidbody API.

## Force-driven movement
Use AddForce when acceleration and momentum are part of the desired feel. Tune force, mass, drag/damping, and maximum speed together. If you continuously overwrite velocity every physics step, force-based acceleration will be overridden.

## Kinematic movement
Kinematic bodies are moved by code and have different collision/force behaviour from Dynamic bodies. Use the documented Rigidbody movement methods and test contact behaviour.

## Common mistakes
- Moving a Dynamic Rigidbody with Transform.position every frame.
- Writing velocity in both Update and FixedUpdate.
- Overwriting Y velocity while applying horizontal movement.
- Using forces and direct velocity assignment without understanding their interaction.
- Forgetting to freeze rotation when a capsule should stay upright.

Reference: [Rigidbody API](https://docs.unity3d.com/ScriptReference/Rigidbody.html)
