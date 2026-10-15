# 2D Joints and Rope Mechanics

Joints connect Rigidbody2D objects with constraints. Use joints when you want the physics solver to manage a connection instead of manually moving both objects.

## Common joint components
- DistanceJoint2D: keeps connected anchors a target distance apart; useful for rope-like links.
- SpringJoint2D: allows spring-like motion with distance/frequency/damping behavior.
- HingeJoint2D: allows relative rotation around an anchor, useful for swinging objects.
- FixedJoint2D: constrains connected objects to a fixed relative pose.
- SliderJoint2D: constrains motion along one axis.
- WheelJoint2D: suspension/wheel-like motion with spring behavior.
- RelativeJoint2D: constrains relative motion with configurable offsets.
- TargetJoint2D: pulls a Rigidbody2D toward a target position.

Exact fields differ by joint and Unity version. Consult each component's Inspector tooltips.

## Distance joint setup
1. Create two GameObjects with Rigidbody2D and Collider2D.
2. Add DistanceJoint2D to one body.
3. Assign Connected Rigidbody or leave it unset for a world connection, depending on the intended behavior.
4. Configure anchor/connected anchor, Auto Configure Connected Anchor, distance, and collision between connected bodies.
5. Test with the body's mass and gravity settings; anchors need to align with the desired attachment points.

## Rope made from linked segments
For a rope/chain, create multiple small bodies with Collider2D and Rigidbody2D, connect adjacent segments with DistanceJoint2D or HingeJoint2D, and use sensible mass/collider sizes. Too many segments increase solver work and jitter risk. Enable collision between linked bodies only if self-collision is necessary.

## Troubleshooting
- Joint pulls toward the wrong point: inspect anchor spaces and Auto Configure Connected Anchor.
- Chain jitters: shorten chain, tune masses, limit collision complexity, and inspect solver settings.
- Object spins unexpectedly: inspect anchor positions and Rigidbody constraints.
- Joint does nothing: body reference missing, component disabled, or connected body setup incorrect.

Official reference: https://docs.unity3d.com/Manual/Joints2D.html
