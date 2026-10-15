# 3D: Animation

## Animator Controller
States reference clips or blend trees; transitions define when to move between them. Common parameters include Speed, Grounded, Direction, and triggers for one-shot actions.

## Blend Trees and root motion
Blend Trees combine motion based on parameters such as speed and direction. Root motion applies displacement authored in the animation; scripted movement drives the character and lets animation follow. Avoid allowing both systems to control the same movement unintentionally.

## Rig import
Humanoid rigs support retargeting when configured correctly; Generic rigs suit other skeletons. Inspect the model import Rig and Animation tabs when avatars or clips fail.

## Debugging
Check the Animator component, controller, avatar, parameter spelling/types, transition conditions, exit time, layer weights, and whether the expected state is active. Use animation events carefully; essential game state should not rely solely on a clip event.

Reference: [Animator API](https://docs.unity3d.com/ScriptReference/Animator.html)
