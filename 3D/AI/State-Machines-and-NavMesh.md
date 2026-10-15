# 3D AI: State Machines and NavMesh Basics

## State machines
Represent behavior explicitly, such as Patrol → Chase → Attack → Return. Each state defines behavior and transition conditions. This is easier to debug than many overlapping booleans.

## NavMesh setup
1. Install the AI Navigation package compatible with the Unity version.
2. Add a NavMeshSurface to the level and configure included layers.
3. Bake the NavMesh.
4. Add NavMeshAgent to the enemy and ensure it begins on the NavMesh.
5. Assign the target Transform.

## Minimal chase agent
Save as NavMeshChase.cs and attach beside NavMeshAgent.

    using UnityEngine;
    using UnityEngine.AI;
    [RequireComponent(typeof(NavMeshAgent))]
    public sealed class NavMeshChase : MonoBehaviour
    {
        [SerializeField] private Transform target;
        [SerializeField, Min(0f)] private float updateInterval = 0.2f;
        private NavMeshAgent agent;
        private float nextUpdate;
        private void Awake() => agent = GetComponent<NavMeshAgent>();
        private void Update()
        {
            if (target == null || !agent.isOnNavMesh) return;
            if (Time.time < nextUpdate) return;
            nextUpdate = Time.time + updateInterval;
            agent.SetDestination(target.position);
        }
    }

This is path-following, not complete AI. Add detection range, line of sight, attack cooldown, loss-of-target behavior, and explicit transitions.

Common mistakes: NavMesh not baked, agent outside it, bad radius/height, missing target, dynamic obstacles not configured, or multiple systems writing destination/Transform.
