# Animator Parameters and Transitions

## Typical state-machine design
A character may have Idle, Walk/Run, Jump, Fall, Land, and Attack states. Avoid creating a separate state for every tiny variation if a Blend Tree or parameter can express it more clearly.

## Parameters
- **Float:** continuous value such as speed.
- **Int:** discrete selection such as weapon type.
- **Bool:** persistent condition such as grounded.
- **Trigger:** one-shot request such as attack.

Names and types must match exactly between the Animator Controller and script.

## Example: drive a speed parameter

```csharp
using UnityEngine;

public sealed class AnimatorSpeedDriver : MonoBehaviour
{
    [SerializeField] private Animator animator;
    [SerializeField] private Rigidbody body;
    private static readonly int SpeedId = Animator.StringToHash("Speed");

    private void Awake()
    {
        if (animator == null) animator = GetComponentInChildren<Animator>();
        if (body == null) body = GetComponent<Rigidbody>();
    }

    private void Update()
    {
        if (animator == null || body == null) return;
        Vector3 horizontalVelocity = body.linearVelocity;
        horizontalVelocity.y = 0f;
        animator.SetFloat(SpeedId, horizontalVelocity.magnitude);
    }
}
```

This example uses the Unity 6 Rigidbody.linearVelocity API. Check the version-specific API if you use an older Editor. For a CharacterController, derive speed from the controller's movement displacement or a movement variable instead.

## Transition settings
- **Conditions:** parameters required to enter the destination state.
- **Has Exit Time:** wait for a normalized point in the current animation before transitioning.
- **Transition Duration:** blends outgoing and incoming motion.
- **Interruption Source:** controls whether and how another transition can interrupt.
- **Layer Weight / Avatar Mask:** affect layered animation.

## Debugging checklist
1. Is the correct Animator Controller assigned?
2. Are parameter names and types identical?
3. Does the transition condition ever become true?
4. Is Exit Time delaying the transition?
5. Is the Animator on the expected layer and active?
6. Is root motion enabled while script movement also changes position?

Reference: [Animator API](https://docs.unity3d.com/ScriptReference/Animator.html)
