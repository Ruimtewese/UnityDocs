# 3D Health, Damage, and Death

## Health component
Save as Health3D.cs and attach to any damageable actor. Other scripts call TakeDamage; death effects or respawn behavior can subscribe to Died.

    using System;
    using UnityEngine;

    public sealed class Health3D : MonoBehaviour
    {
        [SerializeField, Min(1f)] private float maxHealth = 100f;
        public float CurrentHealth { get; private set; }
        public float MaxHealth => maxHealth;
        public bool IsDead { get; private set; }
        public event Action<float, float> HealthChanged;
        public event Action Died;

        private void Awake()
        {
            CurrentHealth = maxHealth;
        }

        private void Start()
        {
            HealthChanged?.Invoke(CurrentHealth, maxHealth);
        }

        public void TakeDamage(float amount)
        {
            if (IsDead || amount <= 0f) return;

            CurrentHealth = Mathf.Max(0f, CurrentHealth - amount);
            HealthChanged?.Invoke(CurrentHealth, maxHealth);

            if (CurrentHealth <= 0f)
            {
                IsDead = true;
                Died?.Invoke();
            }
        }

        public void Heal(float amount)
        {
            if (IsDead || amount <= 0f) return;
            CurrentHealth = Mathf.Min(maxHealth, CurrentHealth + amount);
            HealthChanged?.Invoke(CurrentHealth, maxHealth);
        }
    }

If a UI subscribes in OnEnable before Start, it receives the initial event. If the UI may enable later, read CurrentHealth and MaxHealth immediately after subscribing. Decide deliberately whether healing after death is allowed.

## Damage interface
For interchangeable actors, define an IDamageable interface with TakeDamage(float amount). Weapons can detect this interface, rather than depend on a concrete Health3D class. Unity Inspector serialization of interface references needs a wrapper or UnityEngine.Object reference plus a cast.

## Death flow
Use Died to trigger a death animation, score update, respawn, or destruction. Do not put all of these into Health3D; health owns health state while a game-flow system decides what death means.

## Common mistakes
- Multiple hit callbacks apply repeated damage.
- UI subscribes more than once or fails to unsubscribe.
- Health starts at zero because initialization was skipped.
- HealthChanged is fired but the UI assumes event parameters have a different order.
