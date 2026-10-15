# 2D Unity Reference

Focused guides for Unity 6 2D game development. Most 2D physics code uses Rigidbody2D, Collider2D, and Physics2D; these are distinct from 3D physics types.

## Recommended path
1. [C# essentials for Unity](./Core-Concepts/CSharp-For-Unity.md)
2. [Script lifecycle and architecture](./Core-Concepts/Script-Lifecycle-and-Architecture.md)
3. [Input System setup](./Input/Input-System-Setup.md) · [Touch/mobile input](./Input/Touch-and-Mobile-Input.md)
4. [Platformer controller](./Movement/Platformer-Controller.md) · [Top-down movement](./Movement/Top-Down-Movement.md) · [Moving platforms](./Movement/Moving-Platforms.md)
5. [Components and Inspector reference](./Components/Inspector-Reference.md) · [Inspector attributes and serialization](./Components/Inspector-Attributes-and-Serialization.md)
6. [Physics queries and collision matrix](./Physics/Queries-and-Collision-Matrix.md) · [Physics2D query reference](./Physics/Physics2D-Query-Reference.md) · [Ground checks and masks](./Physics/Ground-Checks-and-Layer-Masks.md)
7. [Sprites, tilemaps, and collision](./Gameplay/Tilemap-Sprites-and-Collision.md) · [Tilemap collision workflow](./Tilemaps/Tilemap-Collision-Workflow.md)
8. [Sprite animation](./Animation/Sprite-Animation-and-Animator.md) · [Flipping and aiming](./Animation/Sprite-Flipping-and-Aiming.md)
9. [Camera follow and pixel-perfect art](./Camera/Camera-Follow-and-Pixel-Perfect.md) · [Sorting layers and pixel art](./Rendering/Sorting-Layers-and-Pixel-Art.md) · [Parallax background](./Rendering/Parallax-Background.md)
10. [Health and HUD](./UI/Health-Bar-and-HUD.md) · [Buttons and pause menus](./UI/Buttons-Menus-and-Pause.md)
11. [Audio setup](./Audio/Audio-Setup-and-Sound-Effects.md)
12. [Projectiles and damage](./Physics/Projectile-and-Damage.md) · [Melee hitboxes](./Physics/Melee-and-Area-Hitboxes.md) · [Damage and knockback](./Gameplay/Damage-Knockback-and-Invulnerability.md)
13. [Pickups and respawning](./Gameplay/Pickups-and-Respawning.md) · [Checkpoints](./Gameplay/Checkpoints-and-Level-Progression.md) · [JSON saving](./Gameplay/Save-Load-JSON.md)
14. [Prefabs and ScriptableObjects](./Core-Concepts/Prefabs-ScriptableObjects-and-Data.md) · [Time and coroutines](./Core-Concepts/Time-Coroutines-and-Invoke.md)
15. [Enemy patrol and chase](./AI/Enemy-Patrol-and-Chase.md) · [Debugging](./Debugging/Debugging-and-Profiling.md) · [Performance/pooling](./Optimization/Performance-and-Object-Pooling.md)

## Core references
- [Coordinates and PPU](./Core-Concepts/Coordinate-Spaces-and-PPU.md)
- [Sorting layers and pixels](./Rendering/Sorting-Layers-and-Pixel-Art.md)
- [Screen bounds and wrapping recipe](./Recipes/Screen-Bounds-and-Wrapping.md)
- [Official 2D manual](https://docs.unity3d.com/Manual/Unity2D.html)

## Architecture reminder
Pick one movement authority per object. For a dynamic Rigidbody2D, use physics APIs; for a CharacterController-style custom actor, control its movement explicitly; avoid multiple scripts changing the same position/velocity. Separate gameplay state from UI presentation.
