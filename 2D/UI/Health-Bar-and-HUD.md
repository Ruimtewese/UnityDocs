# 2D HUD: Health, Text, and Layout

This guide uses uGUI: Canvas, RectTransform, Slider, and TextMeshPro.

## Setup
1. Create a Canvas with Screen Space - Overlay.
2. Set Canvas Scaler to Scale With Screen Size and choose a reference resolution.
3. Add a Slider for the health bar and TextMeshPro - Text (UI) for optional numbers.
4. Place the UI using RectTransform anchor presets.
5. Import TMP essentials if prompted.

## Health state
Save as Health2D.cs. Health owns gameplay state and publishes changes; it doesn't know the HUD layout.

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

        private void Awake() => CurrentHealth = maxHealth;

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

## HUD presenter
Save as HealthBarUI.cs and attach it to a UI object. Assign the health target and Slider in Inspector.

    using UnityEngine;
    using UnityEngine.UI;

    public sealed class HealthBarUI : MonoBehaviour
    {
        [SerializeField] private Health2D health;
        [SerializeField] private Slider slider;

        private void OnEnable()
        {
            if (health != null) health.HealthChanged += UpdateBar;
            Refresh();
        }

        private void OnDisable()
        {
            if (health != null) health.HealthChanged -= UpdateBar;
        }

        private void Refresh()
        {
            if (health == null || slider == null) return;
            slider.maxValue = health.MaxHealth;
            slider.value = health.CurrentHealth;
        }

        private void UpdateBar(float current, float max)
        {
            if (slider == null) return;
            slider.maxValue = max;
            slider.value = current;
        }
    }

If the UI enables after health initialization, Refresh reads current state so it doesn't depend on receiving an old event. The names Health2D and HealthBarUI must match their respective filenames.

## Troubleshooting
Invisible UI: check Canvas mode, sorting order, alpha, scale, and RectTransform. Unclickable buttons: check EventSystem, compatible input module, Graphic Raycaster, Raycast Target, CanvasGroup, and overlapping elements. Don't place world-space UI on a screen-space Canvas by accident.
