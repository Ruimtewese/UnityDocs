# Object pooling for 2D effects

Pooling can reduce repeated Instantiate/Destroy overhead for projectiles, particles, and short-lived effects. Measure before adding complexity.

## Basic pool

```csharp
using System.Collections.Generic;
using UnityEngine;

public sealed class Pool2D : MonoBehaviour
{
    [SerializeField] private GameObject prefab;
    [SerializeField, Min(1)] private int prewarm = 12;
    private readonly Queue<GameObject> pool = new();

    private void Awake()
    {
        if (prefab == null)
        {
            Debug.LogError("Assign a prefab.", this);
            enabled = false;
            return;
        }

        for (int i = 0; i < prewarm; i++)
        {
            GameObject item = Instantiate(prefab, transform);
            item.SetActive(false);
            pool.Enqueue(item);
        }
    }

    public GameObject Get(Vector3 position, Quaternion rotation)
    {
        GameObject item = pool.Count > 0 ? pool.Dequeue() : Instantiate(prefab);
        item.transform.SetPositionAndRotation(position, rotation);
        item.SetActive(true);
        return item;
    }

    public void Return(GameObject item)
    {
        if (item == null) return;
        item.SetActive(false);
        item.transform.SetParent(transform);
        pool.Enqueue(item);
    }
}
```

## Reset state on reuse

A pooled projectile must reset velocity, damage, lifetime, collision state, and any coroutine/timer state when activated. Ensure an object cannot be returned to the pool twice. Consider Unity's ObjectPool<T> API for more advanced projects.
