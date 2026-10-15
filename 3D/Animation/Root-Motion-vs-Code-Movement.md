# 3D Animation: Root Motion vs Code-Driven Movement

## Two movement authorities
- **Code-driven movement:** a controller moves the character; animation follows the movement state. This is predictable for platformers and responsive controls.
- **Root motion:** animation clip displacement moves the character. This can produce natural authored movement, but responsiveness and network/physics integration need care.

Do not let Animator root motion and a controller independently move the same Transform unless you deliberately combine them.

## Animator settings
1. Add Animator and assign the controller.
2. Inspect the Avatar and Apply Root Motion option.
3. Check animation import settings for root transform rotation/position baking.
4. Test transition blending and loop behavior.
5. Decide which component owns position and rotation.

## Inspect root motion
This script logs root motion delta for diagnosis. Attach to the same object as Animator.

    using UnityEngine;

    public sealed class RootMotionInspector : MonoBehaviour
    {
        private void OnAnimatorMove()
        {
            Animator animator = GetComponent<Animator>();
            if (animator == null) return;
            Debug.DrawRay(transform.position, animator.deltaPosition, Color.cyan);
        }
    }

For actual root-motion application, implement OnAnimatorMove intentionally and consider how the CharacterController or Rigidbody should be moved. The diagnostic snippet is not a complete root-motion controller.

## Common problems
- Apply Root Motion enabled while controller also moves the same object.
- Root transforms are baked incorrectly in import settings.
- Animator transitions interrupt locomotion clips.
- Root motion moves the root but collider/controller remains misconfigured.
