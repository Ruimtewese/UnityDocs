# 2D: Animation

## Sprite animation
Create an Animation Clip that changes the SpriteRenderer's Sprite over time. Choose a frame rate that suits the art style and gameplay; do not assume a high frame rate is always better.

## Animator Controller
Use states for idle, run, jump, fall, attack, hurt, and death as appropriate. Parameters commonly include Speed (float), Grounded (bool), and Attack (trigger). Keep transitions understandable and inspect exit time, duration, and conditions when a state change feels delayed.

## Setup checklist
1. Import or slice the sprite sheet.
2. Select the frames in the intended order and create a clip.
3. Confirm the Animator Controller and Animator are assigned.
4. Add parameters and transitions.
5. Drive parameters from gameplay state, not from unrelated visual guesses.

## Common mistakes
Animation events are useful for presentation timing, but avoid making essential game state depend only on an event in a clip that may be edited. Separate sprite flipping from physics direction if they can differ.

Official references: [Animator](https://docs.unity3d.com/ScriptReference/Animator.html) · [2D Animation package](https://docs.unity3d.com/Packages/com.unity.2d.animation@latest)
