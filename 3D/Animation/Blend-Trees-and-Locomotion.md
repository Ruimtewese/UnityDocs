# 3D Animator Blend Trees and Locomotion

A Blend Tree blends motion clips using parameter values instead of many separate transitions. A common locomotion setup blends idle, walk, and run based on Speed. A 2D directional Blend Tree can blend forward/back/strafe clips by horizontal and vertical movement values.

## Setup for 1D locomotion
1. Create an Animator Controller and add a Blend Tree state.
2. Choose a 1D Blend Tree and use a Float parameter named Speed.
3. Add idle, walk, and run clips with thresholds appropriate to your Speed units.
4. Ensure looping is configured on locomotion clips.
5. Drive the parameter from controller velocity or normalized input.

## Parameter driver
Attach to the object containing the Animator. Assign Animator and the Transform/Rigidbody from which speed is measured; this example measures Rigidbody velocity in Unity 6.

    using UnityEngine;

    public sealed class LocomotionBlendDriver : MonoBehaviour
    {
        [SerializeField] private Animator animator;
        [SerializeField] private Rigidbody body;
        [SerializeField, Min(0.01f)] private float speedDampTime = 0.1f;
        private static readonly int SpeedId = Animator.StringToHash("Speed");

        private void Awake()
        {
            if (animator == null) animator = GetComponent<Animator>();
            if (body == null) body = GetComponentInParent<Rigidbody>();
        }

        private void Update()
        {
            if (animator == null || body == null) return;
            Vector3 horizontal = body.linearVelocity;
            horizontal.y = 0f;
            animator.SetFloat(SpeedId, horizontal.magnitude, speedDampTime, Time.deltaTime);
        }
    }

The animation's thresholds must match the units being supplied. If the Rigidbody's velocity is measured in world units per second, thresholds must use comparable values. If using root motion, choose one authority for movement and avoid simultaneous scripted motion.

## Common mistakes
- Parameter name differs from the controller.
- Speed values do not match Blend Tree thresholds.
- Idle clip has an unintended root displacement.
- Blend parameter is fed movement input magnitude but thresholds assume world speed.
- Animator is on a child but code looks only on the root.
