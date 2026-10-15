# C# Events and Interfaces in Unity

Events let a component announce that something happened. Interfaces define a contract shared by different components. Both reduce unnecessary direct dependencies when used carefully.

## Event publisher
Save as EventChannelExample.cs.

    using System;
    using UnityEngine;

    public sealed class EventChannelExample : MonoBehaviour
    {
        public event Action<int> ScoreChanged;

        public void SetScore(int score)
        {
            ScoreChanged?.Invoke(score);
        }
    }

A UI component can subscribe in OnEnable and unsubscribe in OnDisable. If it enables after the last change, it must also obtain the current state separately; events notify changes, not a guaranteed history.

## Interface contract

    public interface IDamageable
    {
        void TakeDamage(float amount);
    }

An enemy Health component and a destructible crate can both implement IDamageable. A weapon then calls the interface rather than requiring one concrete Health class. To look it up from a Collider, obtain MonoBehaviours from the collider or its parents and check which implement the interface.

## Subscription pattern

    private void OnEnable()
    {
        if (publisher != null) publisher.ScoreChanged += OnScoreChanged;
    }

    private void OnDisable()
    {
        if (publisher != null) publisher.ScoreChanged -= OnScoreChanged;
    }

Use the actual publisher field and a method with a matching signature in your component. Avoid adding anonymous event lambdas repeatedly if they cannot later be unsubscribed.

## Common mistakes
- Forgetting to unsubscribe creates duplicate callbacks or stale references.
- Expecting a late subscriber to receive earlier events.
- Event publisher is destroyed before the subscriber cleans up.
- Interface reference is not found because only the hit child component was checked.
- Event code silently assumes a specific script execution order.
