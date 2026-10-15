# 3D Projectiles and Hitscan Weapons

## Hitscan raycast
A hitscan weapon checks along a ray instantly rather than spawning a visible projectile. Save as HitscanWeapon3D.cs and attach to the weapon/player. Requires a Camera reference and optional hitLayers mask.

    using UnityEngine;

    public sealed class HitscanWeapon3D : MonoBehaviour
    {
        [SerializeField] private Camera aimCamera;
        [SerializeField, Min(0f)] private float damage = 20f;
        [SerializeField, Min(0.1f)] private float range = 100f;
        [SerializeField] private LayerMask hitLayers = ~0;
        [SerializeField, Min(0f)] private float cooldown = 0.2f;
        private float nextShotTime;

        public void Fire()
        {
            if (Time.time < nextShotTime || aimCamera == null) return;
            nextShotTime = Time.time + cooldown;

            Ray ray = aimCamera.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0f));
            if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayers,
                    QueryTriggerInteraction.Ignore))
            {
                Health3D health = hit.collider.GetComponentInParent<Health3D>();
                if (health != null) health.TakeDamage(damage);
                Debug.DrawLine(ray.origin, hit.point, Color.red, 0.5f);
            }
            else
            {
                Debug.DrawRay(ray.origin, ray.direction * range, Color.yellow, 0.5f);
            }
        }
    }

Call Fire from an Input System action callback or button. Ensure the weapon's LayerMask includes valid targets but excludes the player if self-hit should not count. Raycasts use world-space distance in Unity units.

## Physics projectile
A physical projectile needs a Collider, a Rigidbody, a speed/launch direction, a lifetime, and collision handling. Set its Rigidbody's collision detection mode appropriately for the speed. For fast projectiles, consider a ray/sphere cast from previous to next position to prevent tunneling.

## Common mistakes
- Ray begins inside a collider that blocks the target.
- Layer mask excludes enemies or includes the player's own colliders.
- Trigger interaction setting doesn't match target colliders.
- A projectile damages the same target more than once before being destroyed.
- Hitscan range and projectile speed are confused; hitscan has no travel time.
