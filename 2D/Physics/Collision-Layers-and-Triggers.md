# Collision layers and triggers (2D)

## Layers vs tags

Use **layers** to control which objects can collide and to filter physics queries. Use **tags** to identify broad gameplay categories such as Player or Enemy. A layer is not a substitute for a gameplay identity.

## Configure collision

1. Create layers such as Player, Ground, Enemy, Hazard, and Collectible.
2. Assign each GameObject to the appropriate layer.
3. Open **Edit > Project Settings > Physics 2D**.
4. Review the Layer Collision Matrix and disable pairs that should never interact.
5. Test triggers and collision callbacks in Play Mode.

## Example: damage zone

Attach to a hazard with a Collider2D whose Is Trigger box is enabled.

```csharp
using UnityEngine;

public sealed class DamageZone2D : MonoBehaviour
{
    [SerializeField, Min(0)] private int damage = 1;

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (!other.CompareTag("Player")) return;

        PlayerHealth health = other.GetComponentInParent<PlayerHealth>();
        if (health != null)
            health.TakeDamage(damage);
    }
}
```

This expects your project to provide a PlayerHealth component with a public TakeDamage(int amount) method. Keep health logic in that component, not in the hazard.

## Debug checklist

- Use Collider2D and Rigidbody2D, not 3D components.
- Verify the trigger checkbox and layer matrix.
- Check whether the callback is OnTriggerEnter2D or OnCollisionEnter2D.
- Confirm the object has the expected tag and component.
- Avoid using string tags for complex systems when a typed component/interface is more appropriate.
