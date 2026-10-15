# 3D Object Pooling

Pooling reuses instantiated objects instead of repeatedly creating and destroying them. It is useful for bullets, impact effects, collectibles, and other short-lived objects with high spawn rates. It may be unnecessary for infrequent objects; profile first.

## Basic pool
Attach to a manager, assign a prefab and initial count, then call Get to spawn and Release to recycle. This minimal version does not prevent a caller from returning an object twice; add ownership/state tracking for production systems.

    using System.Collections.Generic;
    using UnityEngine;

    public sealed class ObjectPool3D : MonoBehaviour
    {
        [SerializeField] private GameObject prefab;
        [SerializeField, Min(0)] private int initialSize = 10;
        private readonly Queue<GameObject> available = new();

        private void Awake()
        {
            if (prefab == null)
            {
                Debug.LogError("Assign a pool prefab.", this);
                enabled = false;
                return;
            }
            for (int i = 0; i < initialSize; i++) CreateOne();
        }

        private GameObject CreateOne()
        {
            GameObject item = Instantiate(prefab, transform);
            item.SetActive(false);
            available.Enqueue(item);
            return item;
        }

        public GameObject Get(Vector3 position, Quaternion rotation)
        {
            if (available.Count == 0) CreateOne();
            GameObject item = available.Dequeue();
            item.transform.SetPositionAndRotation(position, rotation);
            item.SetActive(true);
            return item;
        }

        public void Release(GameObject item)
        {
            if (item == null) return;
            item.SetActive(false);
            item.transform.SetParent(transform);
            available.Enqueue(item);
        }
    }

## Reset on reuse
Pooled objects must reset Rigidbody velocity/angular velocity, lifetime timers, targets, collision flags, particle state, trail renderers, and gameplay state. OnEnable is a common place to start a fresh lifetime, but don't assume every referenced object survives between uses.

## Common mistakes
- Object is released twice and appears twice in the queue.
- Rigidbody keeps velocity from the previous launch.
- Particle/trail renderer retains old visual data.
- Event subscriptions accumulate after repeated enable cycles.
- Pool grows without a maximum or profiling shows pooling was not a bottleneck.
