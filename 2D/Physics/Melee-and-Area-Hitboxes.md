# 2D Melee Attacks and Area Hitboxes

For melee attacks, use a temporary hitbox or an overlap query during the active frames of an attack. Avoid leaving a damage collider active all the time, which can apply repeated hits.

## One-time overlap attack
Attach this script to the player/weapon. Set the attackPoint child Transform and choose the Enemy layer in targetLayers. Trigger Attack from an input action or animation event.

    using System.Collections.Generic;
    using UnityEngine;

    public sealed class MeleeAttack2D : MonoBehaviour
    {
        [SerializeField] private Transform attackPoint;
        [SerializeField] private Vector2 attackSize = new Vector2(1f, 0.7f);
        [SerializeField, Min(0f)] private float damage = 20f;
        [SerializeField] private LayerMask targetLayers;
        private readonly HashSet<Health2D> hitThisSwing = new();

        public void Attack()
        {
            if (attackPoint == null) return;
            hitThisSwing.Clear();

            Collider2D[] hits = Physics2D.OverlapBoxAll(
                attackPoint.position, attackSize, attackPoint.eulerAngles.z, targetLayers);

            foreach (Collider2D hit in hits)
            {
                Health2D health = hit.GetComponentInParent<Health2D>();
                if (health == null || !hitThisSwing.Add(health)) continue;
                health.TakeDamage(damage);
            }
        }

        private void OnDrawGizmosSelected()
        {
            if (attackPoint == null) return;
            Gizmos.color = Color.red;
            Gizmos.matrix = Matrix4x4.TRS(
                attackPoint.position, attackPoint.rotation, Vector3.one);
            Gizmos.DrawWireCube(Vector3.zero, attackSize);
        }
    }

This applies damage immediately once per attack call. For animation-timed attacks, call it from an Animation Event placed during the active swing frames, or use an explicit active hitbox window. OverlapBoxAll allocates an array; for frequent attacks, use a NonAlloc query and handle buffer capacity.

## Common mistakes
- Target layer missing from LayerMask.
- Hit applies multiple times because several child colliders belong to one enemy; the HashSet prevents that within one call.
- Animation event method is on a different object than expected.
- The attack box rotates around the wrong pivot.
- Overlap area is checked continuously instead of only during attack frames.
