# Performance and Object Pooling

Profile first. Common costs include frequent Instantiate/Destroy, per-frame allocations, broad physics queries, oversized textures, transparent overdraw, and frequent UI rebuilds.

## Simple GameObject pool
Attach to a pool manager, assign a prefab, and set initial size. Call Release when an effect/projectile finishes. Do not Destroy pooled instances.

    using System.Collections.Generic;
    using UnityEngine;
    public sealed class SimpleObjectPool : MonoBehaviour
    {
        [SerializeField] private GameObject prefab;
        [SerializeField, Min(0)] private int initialSize = 10;
        private readonly Queue<GameObject> available = new();
        private void Awake()
        {
            if (prefab == null)
            {
                Debug.LogError("Pool prefab is missing.", this);
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

This minimal pool does not prevent releasing an object twice; production pools should track ownership/state. Reset velocity, timers, targets, and gameplay state when reusing items.

## More practices
Cache component references, reuse collections, avoid allocations in tight loops, use layer masks, avoid scene-wide searches in gameplay loops, use sensible asset compression, simplify colliders, and profile the target build.
