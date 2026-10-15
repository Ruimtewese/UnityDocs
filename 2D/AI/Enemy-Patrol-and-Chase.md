# 2D Enemy Patrol and Chase AI

A simple enemy can move between waypoints and chase a target inside a range. This sample uses Rigidbody2D and Transform positions for target detection; it does not implement line-of-sight occlusion.

## Setup
Create an enemy with Rigidbody2D and Collider2D. Assign two waypoint Transforms and a target. Freeze Z rotation if the enemy should stay upright. Choose whether the enemy moves by physics or by Transform; this example uses Rigidbody2D.MovePosition.

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody2D))]
    public sealed class EnemyPatrolChase2D : MonoBehaviour
    {
        [SerializeField] private Transform pointA;
        [SerializeField] private Transform pointB;
        [SerializeField] private Transform target;
        [SerializeField, Min(0f)] private float patrolSpeed = 2f;
        [SerializeField, Min(0f)] private float chaseSpeed = 3f;
        [SerializeField, Min(0f)] private float detectionRange = 5f;

        private Rigidbody2D body;
        private bool towardB = true;

        private void Awake() => body = GetComponent<Rigidbody2D>();

        private void FixedUpdate()
        {
            Vector2 current = body.position;
            bool chasing = target != null &&
                ((Vector2)target.position - current).sqrMagnitude <= detectionRange * detectionRange;

            Vector2 destination;
            float speed;
            if (chasing)
            {
                destination = target.position;
                speed = chaseSpeed;
            }
            else
            {
                if (pointA == null || pointB == null) return;
                destination = towardB ? (Vector2)pointB.position : (Vector2)pointA.position;
                speed = patrolSpeed;
            }

            Vector2 next = Vector2.MoveTowards(current, destination, speed * Time.fixedDeltaTime);
            body.MovePosition(next);

            if (!chasing && Vector2.Distance(next, destination) <= 0.05f)
                towardB = !towardB;
        }
    }

## Extensions
Add a line-of-sight raycast for walls, a return-to-patrol delay, attack range and cooldown, and explicit states when behavior becomes complex. Do not allow a separate movement script to also move the same Rigidbody2D.

## Common mistakes
- Waypoints are missing.
- Rigidbody2D settings conflict with direct Transform changes elsewhere.
- Chase range is checked using distance from the wrong origin.
- Enemy chases through walls because only distance is checked.
