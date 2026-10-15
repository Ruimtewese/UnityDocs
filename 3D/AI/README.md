# 3D: AI and Navigation

## Guides
- [State machines and NavMesh basics](./State-Machines-and-NavMesh.md)
- [Patrol and target detection](./Patrol-and-Detection.md)

## NavMesh workflow
Use an AI Navigation package compatible with the installed Unity version. Add/configure NavMeshSurface where required, bake navigable surfaces, and keep each NavMeshAgent on the baked mesh. Check radius, height, stopping distance, speed, acceleration, and obstacle settings.

## Behavior design
A small finite state machine commonly includes Patrol, Chase, Attack, and Return. Keep state transitions explicit. Use distance for simple awareness and a raycast for line of sight; distance alone allows enemies to detect through walls.

## Troubleshooting
- No baked/valid path or agent is off the NavMesh.
- Target or waypoint is missing.
- Two scripts control the same agent destination or Transform.
- Detection/attack range values use inconsistent world units.
- Frequent recalculation causes unnecessary work.

Official guide: https://docs.unity3d.com/Packages/com.unity.ai.navigation@latest
