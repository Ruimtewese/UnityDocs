# Physics2D Query Reference

Physics2D queries inspect the 2D physics world. Check the layer mask and trigger settings for every query. A LayerMask filters which collider layers the query can hit.

## Common queries
- Physics2D.Raycast: cast a ray in a direction and get the first hit.
- Physics2D.CircleCast: sweep a circle along a direction.
- Physics2D.BoxCast: sweep a box along a direction.
- Physics2D.OverlapCircle: detect colliders overlapping a circle.
- Physics2D.OverlapBox: detect colliders overlapping a box.
- Physics2D.OverlapArea: detect colliders overlapping an axis-aligned area.
- Physics2D.Linecast: test a line segment.
- Collider2D.OverlapPoint: check whether a point lies inside a collider.

## Raycast example

    using UnityEngine;

    public sealed class Raycast2DExample : MonoBehaviour
    {
        [SerializeField, Min(0f)] private float distance = 3f;
        [SerializeField] private LayerMask targetLayers;

        private void Update()
        {
            Vector2 origin = transform.position;
            Vector2 direction = transform.right;
            RaycastHit2D hit = Physics2D.Raycast(origin, direction, distance, targetLayers);
            Debug.DrawRay(origin, direction * distance, hit.collider ? Color.green : Color.red);

            if (hit.collider != null)
                Debug.Log($"Hit {hit.collider.name}", hit.collider);
        }
    }

## Query rules
- Use the query overload that matches the intended maximum distance and layer mask.
- Do not use all-layer queries when you already know the target categories.
- A ray starting inside a collider may behave differently from an approach from outside; test origin placement.
- Decide whether triggers should count as hits.
- For multiple hits, use the appropriate array/list-returning overload and size the buffer correctly.
- For non-alloc queries, handle the possibility that the supplied result buffer fills and truncates results.

## Debugging
Use Debug.DrawRay and draw overlap bounds with Gizmos. Confirm physics layers in Project Settings → Physics 2D. A 2D query only sees Collider2D objects, not 3D Collider components.
