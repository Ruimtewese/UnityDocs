# Coroutines, Events, and Asynchronous Work

## Coroutines
Coroutines spread a sequence across frames. They are not background threads. Use them for cooldowns, timed effects, and waiting for Unity conditions.

    using System.Collections;
    using UnityEngine;
    public sealed class CooldownExample : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float cooldownSeconds = 1f;
        private bool ready = true;
        public void TryUse()
        {
            if (!ready) return;
            ready = false;
            StartCoroutine(ResetCooldown());
        }
        private IEnumerator ResetCooldown()
        {
            yield return new WaitForSeconds(cooldownSeconds);
            ready = true;
        }
    }

WaitForSeconds is affected by Time.timeScale; WaitForSecondsRealtime is not. Coroutines do not parallelize CPU-heavy code, and lifetime depends on the hosting component/GameObject.

## Events
Events let one system notify listeners without directly depending on each listener. A health component can raise Died and UI/audio/game flow can respond. Subscribe in OnEnable and unsubscribe in OnDisable when re-enable cycles are possible.

    using System;
    using UnityEngine;
    public sealed class EventPublisher : MonoBehaviour
    {
        public event Action Changed;
        public void NotifyChanged() => Changed?.Invoke();
    }

Ensure subscribers unsubscribe and event order is deliberate. Avoid retaining stale scene references.

## Async and threads
Use async/await when the Unity API or package supports asynchronous operations. Most UnityEngine object APIs must be used on the main thread. Do not call Transform, GameObject, or other engine APIs from Task.Run worker threads. Cancellation and object lifetime matter across scene changes.

Choose Update for frame logic, FixedUpdate for physics, coroutines for timed main-loop sequences, events for notifications, async/await for supported asynchronous operations, and Jobs/threads only when data/API constraints permit.
