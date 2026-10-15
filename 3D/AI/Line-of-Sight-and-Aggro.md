# 3D AI: Line of Sight and Aggro Range

Distance detects nearby targets, but does not tell whether walls block visibility. Combine a range check with a raycast from an eye point toward the target.

## Detection method
Use this component as a detection helper on an enemy. Assign the eye point and target Transform. The target must be on a layer included in sightLayers if you want the ray to confirm it directly.

    using UnityEngine;

    public sealed class LineOfSight3D : MonoBehaviour
    {
        [SerializeField] private Transform eyePoint;
        [SerializeField] private Transform target;
        [SerializeField, Min(0f)] private float viewRange = 10f;
        [SerializeField] private LayerMask sightLayers = ~0;

        public bool CanSeeTarget()
        {
            if (eyePoint == null || target == null) return false;
            Vector3 offset = target.position - eyePoint.position;
            float distance = offset.magnitude;
            if (distance > viewRange || distance < 0.001f) return false;

            if (Physics.Raycast(eyePoint.position, offset / distance, out RaycastHit hit,
                    distance, sightLayers, QueryTriggerInteraction.Ignore))
            {
                return hit.transform == target || hit.transform.IsChildOf(target);
            }
            return false;
        }

        private void OnDrawGizmosSelected()
        {
            if (eyePoint == null) return;
            Gizmos.color = Color.yellow;
            Gizmos.DrawWireSphere(eyePoint.position, viewRange);
            if (target != null)
                Gizmos.DrawLine(eyePoint.position, target.position);
        }
    }

If your target has multiple child colliders or a root Rigidbody, test the hierarchy logic against the actual target. A line of sight check is an input to AI state; it should not directly decide every attack/chase behavior.

## Common mistakes
- The ray hits the enemy's own collider first.
- Sight layer mask excludes the target.
- Ray origin is inside a wall or character collider.
- Range check uses inconsistent units.
- Detection is called every frame even when an interval would be sufficient.
