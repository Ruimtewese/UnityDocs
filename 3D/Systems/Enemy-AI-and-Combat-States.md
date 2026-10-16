# 3D Enemy AI: Patrol, Chase, Attack, Return

This simple finite-state machine moves directly between patrol points and a player. It does not avoid walls. Use Unity AI Navigation and a NavMeshAgent for obstacle-aware movement.

## EnemyAI3D.cs

~~~csharp
using UnityEngine;

public class EnemyAI3D : MonoBehaviour
{
    private enum State { Patrol, Chase, Attack, Return }
    [SerializeField] private Transform player, pointA, pointB;
    [SerializeField] private MeleeAttack3D attack;
    [SerializeField] private float patrolSpeed = 2, chaseSpeed = 4;
    [SerializeField] private float detectionRange = 8, attackRange = 1.5f, cooldown = 1.2f;
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
        float distance = Vector3.Distance(transform.position, player.position);
        if (distance <= attackRange) state = State.Attack;
        else if (distance <= detectionRange) state = State.Chase;
        else if (state == State.Chase || state == State.Attack) state = State.Return;

        if (state == State.Attack)
        {
            Face(player.position);
            if (Time.time >= nextAttack && attack)
            {
                attack.Attack();
                nextAttack = Time.time + cooldown;
            }
            return;
        }

        Transform destination = state == State.Chase ? player : patrolTarget;
        float speed = state == State.Chase ? chaseSpeed : patrolSpeed;
        MoveToward(destination.position, speed);
        if (state == State.Patrol && Vector3.Distance(transform.position, patrolTarget.position) < 0.2f)
            patrolTarget = patrolTarget == pointA ? pointB : pointA;
        if (state == State.Return && Vector3.Distance(transform.position, patrolTarget.position) < 0.2f)
            state = State.Patrol;
    }

    private void MoveToward(Vector3 point, float speed)
    {
        Vector3 delta = point - transform.position; delta.y = 0;
        if (delta.sqrMagnitude < 0.001f) return;
        Vector3 dir = delta.normalized;
        transform.position += dir * speed * Time.deltaTime;
        Face(transform.position + dir);
    }

    private void Face(Vector3 point)
    {
        Vector3 delta = point - transform.position; delta.y = 0;
        if (delta.sqrMagnitude > 0.001f)
            transform.rotation = Quaternion.LookRotation(delta.normalized);
    }
}
~~~

Assign the player, patrol points, and MeleeAttack3D. The player needs the Player tag. The melee component needs an attack origin and target LayerMask.

### Production upgrades
Use NavMeshAgent for obstacle-aware navigation; do not move the same object with both NavMeshAgent and direct Transform writes. Add line-of-sight raycasts, attack wind-up/recovery, animation events, hit reactions, and a dead state. For many enemies, inject target references rather than repeatedly searching the scene.
