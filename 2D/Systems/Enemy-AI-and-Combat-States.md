# 2D Enemy AI: Patrol, Chase, Attack, Return

This example is a small finite-state machine for simple top-down enemies. It moves directly through the world; it is not pathfinding and will not route around walls.

## EnemyAI2D.cs

~~~csharp
using UnityEngine;

public class EnemyAI2D : MonoBehaviour
{
    private enum State { Patrol, Chase, Attack, Return }
    [SerializeField] private Transform player, pointA, pointB;
    [SerializeField] private MeleeAttack2D attack;
    [SerializeField] private float patrolSpeed = 1.5f, chaseSpeed = 3;
    [SerializeField] private float detectionRange = 5, attackRange = 1, cooldown = 1;
    private Transform patrolTarget;
    private State state = State.Patrol;
    private float nextAttack;

    private void Start()
    {
        if (!player)
        {
            GameObject found = GameObject.FindGameObjectWithTag("Player");
            if (found) player = found.transform;
        }
        patrolTarget = pointA;
    }

    private void Update()
    {
        if (!player || !pointA || !pointB) return;
        float distance = Vector2.Distance(transform.position, player.position);
        if (distance <= attackRange) state = State.Attack;
        else if (distance <= detectionRange) state = State.Chase;
        else if (state == State.Chase || state == State.Attack) state = State.Return;

        if (state == State.Attack)
        {
            if (Time.time >= nextAttack && attack)
            {
                attack.Attack();
                nextAttack = Time.time + cooldown;
            }
            return;
        }

        Transform destination = state == State.Chase ? player : patrolTarget;
        float speed = state == State.Chase ? chaseSpeed : patrolSpeed;
        Vector2 next = Vector2.MoveTowards(transform.position, destination.position, speed * Time.deltaTime);
        transform.position = new Vector3(next.x, next.y, transform.position.z);

        if (state == State.Patrol && Vector2.Distance(transform.position, patrolTarget.position) < 0.12f)
            patrolTarget = patrolTarget == pointA ? pointB : pointA;
        if (state == State.Return && Vector2.Distance(transform.position, patrolTarget.position) < 0.12f)
            state = State.Patrol;
    }
}
~~~

### Setup and upgrades
Enemy needs this script and MeleeAttack2D. Assign player, two patrol points, and attack component. Player needs tag Player. The attack origin and target layer mask must be configured.

- This simple version uses Transform movement. For dynamic Rigidbody2D enemies, move the body in FixedUpdate using MovePosition instead.
- Add Physics2D.Linecast for line-of-sight so walls block detection.
- Add attack wind-up/recovery, animation events, and a dead state.
- For platformers, add ledge checks and jump planning or use a platformer pathfinding solution.
