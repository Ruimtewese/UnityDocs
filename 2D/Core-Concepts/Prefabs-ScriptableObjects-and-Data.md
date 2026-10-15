# Prefabs, ScriptableObjects, and Data

## Prefabs
A prefab is a reusable serialized GameObject hierarchy. Drag a configured scene object into the Project window to create one. Use prefabs for enemies, projectiles, pickups, effects, and repeatable UI.

    using UnityEngine;
    public sealed class Spawner2D : MonoBehaviour
    {
        [SerializeField] private GameObject prefab;
        [SerializeField] private Transform spawnPoint;
        public GameObject Spawn()
        {
            if (prefab == null)
            {
                Debug.LogError("Assign a prefab in the Inspector.", this);
                return null;
            }
            Transform point = spawnPoint != null ? spawnPoint : transform;
            return Instantiate(prefab, point.position, point.rotation);
        }
    }

Instantiate creates a runtime instance; it does not create a Project asset.

## Prefab workflow
- Edit the prefab asset when every instance should inherit the change.
- Inspect Overrides for scene-specific differences.
- Use variants for related types that share a base.
- Avoid scene-only references in reusable prefab assets unless assigned at runtime.
- Keep required components and dependencies on the prefab where practical.

## ScriptableObjects
Use ScriptableObjects for shared authored definitions such as weapons, enemies, levels, and items.

    using UnityEngine;
    [CreateAssetMenu(menuName = "Game/Weapon Definition")]
    public sealed class WeaponDefinition : ScriptableObject
    {
        public string displayName = "Basic";
        [Min(0f)] public float damage = 10f;
        [Min(0f)] public float cooldown = 0.5f;
        public Sprite icon;
    }

Create the asset through the Project window's Create menu. ScriptableObject assets are shared authored data, not a player save system. Avoid mutating project asset values as if they were per-player progress.

## Choosing storage
- Serialized component fields: per-instance configuration.
- Prefab: reusable object/component setup.
- ScriptableObject: shared authored definitions.
- JSON or platform save API: runtime player progress.
- Static fields/singletons: convenient global access but can hide dependencies and complicate testing.
