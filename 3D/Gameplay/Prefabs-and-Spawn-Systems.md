# 3D Prefabs and Spawn Systems

Build an object in the scene, add required components, configure collider/Rigidbody settings, test it, then drag the root into the Project window to create a prefab. Use prefab variants for related objects that share a base.

## Spawner
Save as PrefabSpawner3D.cs and assign a prefab and optional spawn point.

    using UnityEngine;
    public sealed class PrefabSpawner3D : MonoBehaviour
    {
        [SerializeField] private GameObject prefab;
        [SerializeField] private Transform spawnPoint;
        public GameObject Spawn()
        {
            if (prefab == null)
            {
                Debug.LogError("PrefabSpawner3D: assign a prefab.", this);
                return null;
            }
            Transform point = spawnPoint != null ? spawnPoint : transform;
            return Instantiate(prefab, point.position, point.rotation);
        }
    }

## Spawn-system considerations
- Validate random spawn positions against bounds and obstacles.
- Spawned Rigidbody objects may need velocity/state reset if pooled.
- Avoid spawning inside walls or the player.
- Bound spawn frequency and maximum active count.
- Use pooling for high-frequency projectiles and effects.
- Avoid scene-instance dependencies on prefab assets; assign scene references at runtime.
