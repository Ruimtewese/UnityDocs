# 2D Moving Platforms

Use Rigidbody2D for a platform that moves through the physics world. Add a BoxCollider2D and Rigidbody2D set to Kinematic. Assign two endpoint Transforms in the Inspector; this script caches their world positions at startup, so the endpoint objects may be children without following the platform forever.

## Back-and-forth platform
Save as MovingPlatform2D.cs and attach to the platform.

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody2D))]
    public sealed class MovingPlatform2D : MonoBehaviour
    {
        [SerializeField] private Transform pointA;
        [SerializeField] private Transform pointB;
        [SerializeField, Min(0f)] private float speed = 2f;

        private Rigidbody2D body;
        private Vector2 positionA;
        private Vector2 positionB;
        private bool endpointsValid;
        private bool movingToB = true;

        private void Awake()
        {
            body = GetComponent<Rigidbody2D>();
            endpointsValid = pointA != null && pointB != null;
            if (endpointsValid)
            {
                positionA = pointA.position;
                positionB = pointB.position;
            }
            else
            {
                Debug.LogError("Assign both platform endpoints.", this);
            }
        }

        private void FixedUpdate()
        {
            if (!endpointsValid) return;

            Vector2 target = movingToB ? positionB : positionA;
            Vector2 next = Vector2.MoveTowards(
                body.position, target, speed * Time.fixedDeltaTime);
            body.MovePosition(next);

            if (Vector2.Distance(next, target) < 0.01f)
                movingToB = !movingToB;
        }
    }

Set Rigidbody2D Body Type to Kinematic. Put endpoint Transforms where the platform's center should stop. MovePosition in FixedUpdate is preferable to writing Transform position each frame for a physics object.

## Carrying the player
First test Rigidbody2D interpolation and collider behavior. Common strategies include parenting while grounded or applying platform delta to player movement. Applying platform delta through the movement controller avoids many hierarchy issues, but needs integration with the player controller. Avoid parenting/unparenting every frame due to tiny grounding fluctuations.

## Common mistakes
- Endpoints are unassigned or accidentally placed at the same world position.
- The platform moves in Update while the player simulates in FixedUpdate.
- Player slides off due to friction or controller code.
- Collider shapes snag at edges.
