# Health and damage system (2D)

This small reusable component tracks health, exposes a damage method, and raises an event when health changes. It does not assume a particular UI or respawn system.

## PlayerHealth.cs

```csharp
using System;
using UnityEngine;

public sealed class PlayerHealth : MonoBehaviour
{
    [SerializeField, Min(1)] private int maxHealth = 5;
    public int CurrentHealth { get; private set; }
    public int MaxHealth => maxHealth;
    public bool IsDead => CurrentHealth <= 0;

    public event Action<int, int> HealthChanged;
    public event Action Died;

    private void Awake()
    {
        CurrentHealth = maxHealth;
        HealthChanged?.Invoke(CurrentHealth, maxHealth);
    }

    public void TakeDamage(int amount)
    {
        if (IsDead || amount <= 0) return;

        CurrentHealth = Mathf.Max(0, CurrentHealth - amount);
        HealthChanged?.Invoke(CurrentHealth, maxHealth);

        if (IsDead)
            Died?.Invoke();
    }

    public void Heal(int amount)
    {
        if (IsDead || amount <= 0) return;

        CurrentHealth = Mathf.Min(maxHealth, CurrentHealth + amount);
        HealthChanged?.Invoke(CurrentHealth, maxHealth);
    }

    public void RestoreFullHealth()
    {
        CurrentHealth = maxHealth;
        HealthChanged?.Invoke(CurrentHealth, maxHealth);
    }
}
```

## Connecting a UI

Subscribe to HealthChanged in the UI's OnEnable and unsubscribe in OnDisable. Update the bar only when the event fires, rather than searching for UI objects every frame. Subscribe to Died to open a game-over screen or request a respawn.

## Design notes

- Clamp health so it never falls below zero or above maximum.
- Ignore damage after death unless your game explicitly supports it.
- Decide whether healing can revive a dead character.
- For multiplayer, validate damage and health changes on the authoritative peer/server.
