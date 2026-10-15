# 2D: Animation

## Guides
- [Sprite animation and Animator](./Sprite-Animation-and-Animator.md)
- [Sprite flipping and aiming](./Sprite-Flipping-and-Aiming.md)

## Workflow
Import the sheet as Sprite (2D and UI), slice sprites in Sprite Editor, and create animation clips. Use an Animator Controller when states/parameters/transitions are useful; direct sprite assignment may suffice for very simple animations.

## Animator checks
Animator references the correct controller, clips are present in states, transition conditions refer to existing parameters with matching types, looping clips have Loop Time enabled, and no two systems control the same Transform property.

## Sprite consistency
Keep Pixels Per Unit and pivots consistent between frames. Use SpriteRenderer sorting/layers independently of physics layers. Test flipping with child weapons/colliders so visuals and hitboxes remain aligned.
