# 3D Joints and Rigidbody Constraints

Joints connect Rigidbody objects and limit how they move relative to one another. Constraints instead freeze selected axes on a single Rigidbody.

## Common joint types
- FixedJoint: locks relative motion.
- HingeJoint: allows rotation around one axis.
- SpringJoint: allows spring-like separation behavior.
- ConfigurableJoint: advanced control of linear/angular motion and drives.
- CharacterJoint: specialized joint commonly used in ragdoll limbs.

Joint settings vary by type. Read the component tooltips and test with the actual mass scale and force range.

## Simple hinge setup
1. Add Rigidbody and Collider to a moving object and ensure the hinge object has an appropriate Rigidbody or fixed attachment.
2. Add HingeJoint to the moving object.
3. Set Connected Body if joining another Rigidbody; leave empty when attaching to the world.
4. Configure Anchor, Axis, Use Limits, limits, and motor only as needed.
5. Test the anchor location in Scene view; a misplaced anchor creates an unexpected pivot.

## Rigidbody constraints
Use Freeze Position and Freeze Rotation axes to prevent unwanted degrees of freedom. For a character that must remain upright, freezing X/Z rotation is common in a Y-up scene. Don't freeze axes that the controller expects to move.

## Troubleshooting
- Joint explodes or jitters: check mass ratios, intersecting colliders, anchor positions, solver settings, and excessive force.
- Hinge rotates around the wrong point: adjust Anchor and axis orientation.
- Joint doesn't move: connected body is missing, joint is disabled, constraints conflict, or the object is kinematic.
- Objects pass through each other: inspect collider shape, layers, and collision detection.

Official reference: https://docs.unity3d.com/Manual/Joints.html
