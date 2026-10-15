# 2D HUD: Health, Text, and Layout

This guide uses uGUI: Canvas, RectTransform, Image, Slider, and TextMeshPro.

## Setup
Create a Canvas, use Screen Space - Overlay for a standard HUD, and set Canvas Scaler to Scale With Screen Size. Choose a reference resolution. Add a Slider or Image for health and TextMeshPro text for numbers. Use RectTransform anchor presets for responsive placement. Import TMP essentials if prompted.

## Health component
Save as Health2D.cs. It owns health state, not UI presentation.

    using System;
    using UnityEngine;
    public sealed class Health2D : MonoBehaviour
    {
        [SerializeField, Min(1f)] private float maxHealth = 100f;
        public float CurrentHealth { get; private set; }
        public float MaxHealth => maxHealth;
        public event Action<float, float> HealthChanged;
        public event Action Died;
        private bool isDead;
        private void Awake()
        {
            CurrentHealth = maxHealth;
            HealthChanged?.Invoke(CurrentHealth, maxHealth);
        }
        public void TakeDamage(float amount)
        {
            if (isDead || amount <= 0f) return;
            CurrentHealth = Mathf.Max(0f, CurrentHealth - amount);
            HealthChanged?.Invoke(CurrentHealth, maxHealth);
            if (CurrentHealth <= 0f)
            {
                isDead = true;
                Died?.Invoke();
            }
        }
        public void Heal(float amount)
        {
            if (isDead || amount <= 0f) return;
            CurrentHealth = Mathf.Min(maxHealth, CurrentHealth + amount);
            HealthChanged?.Invoke(CurrentHealth, maxHealth);
        }
    }

A separate UI presenter should subscribe to HealthChanged, set Slider maximum/value, and unsubscribe when disabled. Assign references in Inspector. If UI enables after initialization, request current state once or publish after subscription.

## Troubleshooting
Invisible UI: check Canvas mode, sorting order, alpha, scale, and RectTransform. Unclickable buttons: check EventSystem, input module, Graphic Raycaster, Raycast Target, CanvasGroup, and overlapping UI. Do not put world-space UI on a screen-space Canvas by accident.
