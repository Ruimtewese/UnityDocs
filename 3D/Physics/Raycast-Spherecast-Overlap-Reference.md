# 3D Physics Query Reference

Use 3D Physics queries to detect objects, check clearance, target weapons, and test movement. Queries use Collider/Rigidbody objects, not 2D physics components.

## Common queries
- Physics.Raycast: line-like ray with near-zero radius.
- Physics.SphereCast: sweep a sphere; useful for camera collision and rough clearance.
- Physics.BoxCast: sweep a box.
- Physics.CapsuleCast: sweep a capsule, useful for character-clearance tests.
- Physics.OverlapSphere / OverlapBox / OverlapCapsule: find colliders overlapping a volume.
- Physics.CheckSphere / CheckBox / CheckCapsule: return a bool for occupancy checks.
- Collider.Raycast: cast constrained to a specific collider.

## Raycast example

    using UnityEngine;

    public sealed class RaycastReferenceExample : MonoBehaviour
    {
        [SerializeField] private Camera aimCamera;
        [SerializeField, Min(0.01f)] private float range = 50f;
        [SerializeField] private LayerMask layers = ~0;

        public bool TryGetHit(out RaycastHit hit)
        {
            hit = default;
            if (aimCamera == null) return false;
            Ray ray = aimCamera.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0f));
            return Physics.Raycast(ray, out hit, range, layers,
                QueryTriggerInteraction.Ignore);
        }
    }

## Query parameters
Specify maximum distance and a LayerMask whenever possible. QueryTriggerInteraction.UseGlobal follows project settings; Ignore and Collide explicitly select trigger behavior. Inspect the Hit point, normal, distance, collider, and transform when responding.

## Common mistakes
- Mask excludes intended targets.
- Query hits the player's own collider.
- Start position is embedded in geometry.
- Trigger handling differs from Physics settings.
- NonAlloc result buffer is too small.
- A cast is used when an overlap or simple boolean check is sufficient.
