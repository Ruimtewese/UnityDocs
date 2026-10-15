# 2D Moving Platforms

Use a Rigidbody2D for a platform that should move through the physics world. Create a platform with a BoxCollider2D and Rigidbody2D set to Kinematic, and configure its movement path. Parenting a physics player to a platform can introduce edge cases; test the chosen carry approach carefully.

## Back-and-forth platform
Save as MovingPlatform2D.cs and attach to the platform. Set two world-space endpoints using child Transforms.

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody2D))]
    public sealed class MovingPlatform2D : MonoBehaviour
    {
        [SerializeField] private Transform pointA;
        [SerializeField] private Transform pointB;
        [SerializeField, Min(0f)] private float speed = 2f;
        private Rigidbody2D body;
        private bool movingToB = true;

        private void Awake() => body = GetComponent<Rigidbody2D>();

        private void FixedUpdate()
        {
            if (pointA == null || pointB == null) return;
            Vector2 target = movingToB ? pointB.position : pointA.position;
            Vector2 next = Vector2.MoveTowards(body.position, target, speed * Time.fixedDeltaTime);
            body.MovePosition(next);

            if (Vector2.Distance(next, target) < 0.01f)
                movingToB = !movingToB;
        }
    }

Set Rigidbody2D Body Type to Kinematic and assign endpoints. MovePosition in FixedUpdate is preferable to writing a dynamic body's Transform every frame.

## Player interaction
To carry the player reliably, first test Rigidbody2D interpolation and collider behavior. Common strategies include parenting while grounded or applying platform delta to player movement. Avoid parenting/unparenting every frame due to small grounding fluctuations.

Common mistakes: endpoints unassigned or overlapping, platform moves in Update while a Rigidbody player simulates in FixedUpdate, player slides off due to friction/movement code, and collider shapes snag at edges.
