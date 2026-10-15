# 3D AI: Patrol and Target Detection

This example patrols between assigned NavMesh waypoints and chases a target within a simple distance threshold. It requires the AI Navigation setup, baked NavMesh, NavMeshAgent, and explicit waypoints.

## Patrol/chase agent
Save as PatrolChaseAgent.cs and attach beside NavMeshAgent.

    using UnityEngine;
    using UnityEngine.AI;

    [RequireComponent(typeof(NavMeshAgent))]
    public sealed class PatrolChaseAgent : MonoBehaviour
    {
        [SerializeField] private Transform[] patrolPoints;
        [SerializeField] private Transform target;
        [SerializeField, Min(0f)] private float detectionRange = 8f;
        [SerializeField, Min(0.1f)] private float destinationRefresh = 0.2f;
        private NavMeshAgent agent;
        private int pointIndex;
        private float nextRefresh;
        private bool chasing;

        private void Awake() => agent = GetComponent<NavMeshAgent>();

        private void Update()
        {
            if (!agent.isOnNavMesh) return;

            bool canSeeTarget = target != null &&
                (target.position - transform.position).sqrMagnitude <= detectionRange * detectionRange;
            if (canSeeTarget != chasing)
            {
                chasing = canSeeTarget;
                nextRefresh = 0f;
            }

            if (Time.time < nextRefresh) return;
            nextRefresh = Time.time + destinationRefresh;

            if (chasing)
            {
                agent.SetDestination(target.position);
                return;
            }

            if (patrolPoints == null || patrolPoints.Length == 0) return;
            Transform point = patrolPoints[pointIndex];
            if (point == null) return;

            if (!agent.pathPending && agent.remainingDistance <= agent.stoppingDistance + 0.15f)
                pointIndex = (pointIndex + 1) % patrolPoints.Length;

            if (patrolPoints[pointIndex] != null)
                agent.SetDestination(patrolPoints[pointIndex].position);
        }
    }

This uses distance only, not line of sight. Add a raycast between enemy and target if walls should block detection. Do not use a zero stopping distance if the agent continually overshoots narrow waypoints; tune agent radius, speed, acceleration, and stopping distance.

## Common mistakes
- NavMesh has not been baked or agent starts off the mesh.
- A waypoint is null.
- Target detection ignores walls because no line-of-sight test exists.
- Agent destination is changed every frame without a reason.
- Another script drives the same Transform while NavMeshAgent is active.
