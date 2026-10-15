# 2D Projectiles, Damage, and Hit Detection

## Setup
Create a projectile prefab with a SpriteRenderer, CircleCollider2D (usually Is Trigger), and this script. If using trigger callbacks, ensure the projectile or target setup includes a Rigidbody2D as required by Unity's 2D physics. Assign the prefab to the weapon. Set its layer and verify Physics 2D collision filtering.

## Projectile script
Save as Projectile2D.cs and attach to the projectile prefab.

    using UnityEngine;

    public sealed class Projectile2D : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float speed = 12f;
        [SerializeField, Min(0f)] private float damage = 10f;
        [SerializeField, Min(0.1f)] private float lifetime = 3f;
        [SerializeField] private LayerMask hitLayers = ~0;

        private Vector2 direction;
        private GameObject owner;

        public void Initialize(Vector2 travelDirection, GameObject source)
        {
            direction = travelDirection.sqrMagnitude > 0f
                ? travelDirection.normalized
                : Vector2.right;
            owner = source;
            Destroy(gameObject, lifetime);
        }

        private void Update()
        {
            transform.position += (Vector3)(direction * speed * Time.deltaTime);
        }

        private void OnTriggerEnter2D(Collider2D other)
        {
            if (owner != null && other.gameObject == owner) return;
            if ((hitLayers.value & (1 << other.gameObject.layer)) == 0) return;

            Health2D health = other.GetComponentInParent<Health2D>();
            if (health != null) health.TakeDamage(damage);
            Destroy(gameObject);
        }
    }

This simple version moves by Transform and may tunnel at high speeds; use Rigidbody2D movement or a cast per step for fast projectiles. The Health2D component must exist on the target or a parent. Be deliberate about whether a projectile should disappear on hitting non-damageable objects.

## Basic firing script
Attach to a weapon object. The Input System callback may call Fire().

    using UnityEngine;

    public sealed class ProjectileWeapon2D : MonoBehaviour
    {
        [SerializeField] private Projectile2D projectilePrefab;
        [SerializeField] private Transform muzzle;
        [SerializeField, Min(0.01f)] private float cooldown = 0.25f;
        private float nextFireTime;

        public void Fire(Vector2 direction)
        {
            if (Time.time < nextFireTime || projectilePrefab == null) return;
            nextFireTime = Time.time + cooldown;
            Transform origin = muzzle != null ? muzzle : transform;
            Projectile2D projectile = Instantiate(
                projectilePrefab, origin.position, Quaternion.identity);
            projectile.Initialize(direction, gameObject);
        }
    }

## Common mistakes
- Prefab reference is missing.
- Projectile collides with its owner due to incorrect owner/layer filtering.
- Trigger callback never runs because the colliders/Rigidbody2D/layers are incompatible.
- Projectile is aimed using local right when the sprite's forward direction is different.
- Destroy is called immediately on non-target scenery when that is not intended.
- Moving very fast projectiles by Transform causes tunneling.
