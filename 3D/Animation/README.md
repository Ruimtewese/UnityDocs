# 3D: Animation

## Guides
- [Animator parameters and transitions](./Animator-Parameters-and-Transitions.md)
- [Blend Trees and locomotion](./Blend-Trees-and-Locomotion.md)
- [Animation Events](./Animation-Events.md)
- [Root motion versus code-driven movement](./Root-Motion-vs-Code-Movement.md)

## Workflow
Import and configure the rig/Avatar, create clips, make an Animator Controller, add typed parameters, then connect states and transitions. Keep parameter names and types consistent with scripts. Use a Blend Tree for locomotion blending when it simplifies transitions.

## Root motion
Decide if animation-authored displacement or controller code owns movement. Avoid both independently moving the same character. Test clip interruption because animation events that close hitboxes may not run if a clip is interrupted.

## Troubleshooting
Check Avatar/rig mapping, clip ranges/Loop Time, Animator Controller assignment, parameter names/types, transition exit time, and whether the animated property is also written by gameplay code.
