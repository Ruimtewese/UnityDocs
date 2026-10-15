# 2D: AI

## Guide
- [Enemy patrol and chase](./Enemy-Patrol-and-Chase.md)

## Common patterns
Use waypoint patrols for predictable movement, distance tests for basic detection, and Physics2D raycasts for line of sight. Add explicit states as behavior grows: Patrol, Chase, Attack, Return, and Stunned are common examples.

## Debugging
Draw the detection ray, show waypoint gizmos, validate layer masks, and ensure the AI does not compete with another controller for the same Rigidbody2D. Distance-only detection sees through walls unless you test occlusion.
