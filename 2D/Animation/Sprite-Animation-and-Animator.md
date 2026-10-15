# 2D Sprite Animation and Animator

## Import and create clips
1. Set sprite sheet Texture Type to Sprite (2D and UI), Sprite Mode to Multiple, then Apply.
2. Open Sprite Editor, slice the sheet, then Apply.
3. Select sliced sprites in order and create an animation clip.
4. Keep Pixels Per Unit and sprite pivots consistent to prevent size changes and wobble.
5. Inspect Animation window keyframes and remove accidental keys on properties the clip should not control.

## Animator setup
Add Animator and assign an Animator Controller. Create parameters such as Speed (Float), Grounded (Bool), and Jump (Trigger). Use explicit transition conditions. Disable Has Exit Time for immediate locomotion changes; enable it when a clip should finish first. A Blend Tree can smoothly blend idle/walk/run. Names and types must match exactly.

## Parameter driver
Save as SpriteAnimatorDriver.cs and attach to the Animator object.

    using UnityEngine;
    public sealed class SpriteAnimatorDriver : MonoBehaviour
    {
        [SerializeField] private Animator animator;
        private static readonly int SpeedHash = Animator.StringToHash("Speed");
        private void Awake()
        {
            if (animator == null) animator = GetComponent<Animator>();
        }
        public void SetMoveSpeed(float speed)
        {
            if (animator != null) animator.SetFloat(SpeedHash, Mathf.Abs(speed));
        }
        public void SetGrounded(bool grounded)
        {
            if (animator != null) animator.SetBool("Grounded", grounded);
        }
        public void TriggerJump()
        {
            if (animator != null) animator.SetTrigger("Jump");
        }
    }

This drives parameters; it does not create states or transitions. Configure those in the Animator window.

## Sorting and troubleshooting
SpriteRenderer uses Sorting Layer and Order in Layer. For top-down games, sorting by Y may be useful. Check for missing controllers, misspelled parameters, wrong parameter types, disabled Loop Time, inconsistent pivots, and Animator/gameplay code both writing the same Transform property.
