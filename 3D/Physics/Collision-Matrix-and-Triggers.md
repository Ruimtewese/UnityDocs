# 3D collision matrix and triggers

## Collider types

Box, Sphere, Capsule, and Mesh Colliders cover common shapes. Prefer simple colliders for moving objects when possible; complex Mesh Colliders can be more expensive and have restrictions.

## Triggers

Enable **Is Trigger** to detect overlap without physically blocking movement. Use OnTriggerEnter, OnTriggerStay, and OnTriggerExit for 3D physics. The 2D equivalents have a 2D suffix and use different component types.

## Example: pickup trigger

```csharp
using UnityEngine;

public sealed class PickupTrigger : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (!other.CompareTag("Player")) return;

        // Notify inventory or score system here.
        gameObject.SetActive(false);
    }
}
```

Tag the player correctly and ensure the pickup collider is configured as a trigger.

## Layer collision matrix

Create meaningful layers and disable interactions that are never needed. Use layer masks for raycasts and overlap queries. This improves clarity and can reduce unnecessary physics work.

## Troubleshooting

- Callback does not fire: inspect collider enabled state, Is Trigger, Rigidbody presence, layers, and the correct 2D/3D callback signature.
- Object falls through: review collision detection mode and object speed.
- Physics is unstable: avoid teleporting dynamic rigidbodies by writing Transform position each frame.
