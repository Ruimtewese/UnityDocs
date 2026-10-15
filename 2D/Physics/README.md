# 2D: Physics

## Rigidbody2D body types
- **Dynamic:** participates in force/gravity simulation and responds to collisions.
- **Kinematic:** controlled by code and not driven by forces in the same way as Dynamic.
- **Static:** represents geometry that does not move under simulation.

## Collision and trigger callbacks
- Solid contacts: OnCollisionEnter2D, OnCollisionStay2D, OnCollisionExit2D.
- Trigger overlaps: OnTriggerEnter2D, OnTriggerStay2D, OnTriggerExit2D.
- Query APIs include Physics2D.Raycast, CircleCast, BoxCast, and OverlapCircle.

## Materials and filtering
Physics Material 2D controls friction and bounciness. The Physics 2D Layer Collision Matrix decides which layers can interact. Use LayerMasks for queries so ground checks and targeting ignore irrelevant objects.

## Callback troubleshooting
1. Confirm both colliders are enabled and overlap as expected.
2. Check trigger flags and Rigidbody2D setup.
3. Check layer collision settings.
4. Confirm the GameObjects and scripts are active.
5. Check that the method name and signature match Unity's callback exactly.

Use physics-step timing for physics-driven movement and avoid teleporting bodies repeatedly through solid geometry. Reference: [Physics2D API](https://docs.unity3d.com/ScriptReference/Physics2D.html)
