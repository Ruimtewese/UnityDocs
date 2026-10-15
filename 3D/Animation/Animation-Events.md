# 3D Animation Events

Animation Events call a method at a specific frame/time in an Animation Clip. They are useful for footsteps, weapon hit windows, reload stages, and other moments tightly synchronized to a clip.

## Workflow
1. Select a clip and open the Animation window.
2. Move the playhead to the desired frame and add an Event.
3. Choose a method available on the animated GameObject or its relevant receiver setup.
4. Ensure the method name and supported parameter type match the event.
5. Test transitions, interruptions, and looping.

## Example receiver
Attach to the GameObject that receives the Animation Event.

    using UnityEngine;

    public sealed class AnimationEventReceiver : MonoBehaviour
    {
        public void PlayFootstep()
        {
            // Request a footstep sound through the project's audio system.
        }

        public void OpenDamageWindow()
        {
            // Enable the weapon hitbox or record the start of an attack window.
        }

        public void CloseDamageWindow()
        {
            // Disable the weapon hitbox or record the end of the attack window.
        }

        public void AnimationFinished()
        {
            // Notify gameplay that this clip reached a designated event.
        }
    }

These methods are event entry points; connect them to actual project systems. Avoid putting critical one-time game logic only in a clip event if the animation can be interrupted or skipped. Pair animation timing with gameplay state validation.

## Common failures
- Method exists on another GameObject and the expected receiver cannot find it.
- Method is private/incompatible with the event binding behavior.
- Clip/Animator uses a different asset than the one edited.
- Interrupted animation leaves a hitbox enabled because the closing event never runs.
- Event duplicates damage because hit tracking isn't reset per attack.
