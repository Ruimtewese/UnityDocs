# Health and damage system (3D)

This component separates health state from collision, weapons, UI, and animation.

## Health3D.cs

```csharp
using System;
using UnityEngine;

public sealed class Health3D : MonoBehaviour
{
    [SerializeField, Min(1)] private int maxHealth = 100;
    public int CurrentHealth { get; private set; }
    public int MaxHealth => maxHealth;
    public bool IsDead => CurrentHealth <= 0;

    public event Action<int, int> HealthChanged;
    public event Action Died;

    private void Awake()
    {
        CurrentHealth = maxHealth;
    }

    public void TakeDamage(int amount)
    {
        if (IsDead || amount <= 0) return;

        CurrentHealth = Mathf.Max(0, CurrentHealth - amount);
        HealthChanged?.Invoke(CurrentHealth, maxHealth);

        if (IsDead) Died?.Invoke();
    }

    public void Heal(int amount)
    {
        if (IsDead || amount <= 0) return;

        CurrentHealth = Mathf.Min(maxHealth, CurrentHealth + amount);
        HealthChanged?.Invoke(CurrentHealth, maxHealth);
    }
}
```

## Integration

Call TakeDamage from a collision, hazard, or gameplay event. Subscribe the UI to HealthChanged and subscribe a game manager to Died. Keep the component that detects an impact separate from the component that owns health.

## Production considerations

- Decide whether damage can be blocked, reduced, or ignored during invulnerability.
- Use damage data/structs if damage types and sources become complex.
- Avoid relying on object names to identify damage recipients.
- For networked games, health changes should be validated by the authoritative server/host.
