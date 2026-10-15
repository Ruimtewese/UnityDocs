# 2D Unity Reference

Focused guides for Unity 6 2D game development. Most 2D physics code uses Rigidbody2D, Collider2D, and Physics2D, distinct from their 3D counterparts.

## Recommended path
1. [C# essentials](./Core-Concepts/CSharp-For-Unity.md) · [Script lifecycle](./Core-Concepts/Script-Lifecycle-and-Architecture.md)
2. [Input System](./Input/Input-System-Setup.md) · [Touch/mobile input](./Input/Touch-and-Mobile-Input.md)
3. [Platformer controller](./Movement/Platformer-Controller.md) · [Jump buffering/coyote time](./Movement/Jump-Buffer-and-Coyote-Time.md) · [Moving platforms](./Movement/Moving-Platforms.md)
4. [Top-down movement](./Movement/Top-Down-Movement.md) · [Ground checks](./Physics/Ground-Checks-and-Layer-Masks.md)
5. [Component Inspector reference](./Components/Inspector-Reference.md) · [Inspector attributes/serialization](./Components/Inspector-Attributes-and-Serialization.md)
6. [Physics query reference](./Physics/Physics2D-Query-Reference.md) · [Collision matrix](./Physics/Queries-and-Collision-Matrix.md) · [One-way platforms](./Physics/One-Way-Platforms.md) · [Physics materials](./Physics/Physics-Materials-and-Friction.md)
7. [Sprites and tilemaps](./Gameplay/Tilemap-Sprites-and-Collision.md) · [Tilemap collision workflow](./Tilemaps/Tilemap-Collision-Workflow.md)
8. [Sprite animation](./Animation/Sprite-Animation-and-Animator.md) · [Flip and aim](./Animation/Sprite-Flipping-and-Aiming.md)
9. [Camera follow](./Camera/Camera-Follow-and-Pixel-Perfect.md) · [Camera bounds](./Camera/Camera-Bounds.md) · [Sorting/pixel art](./Rendering/Sorting-Layers-and-Pixel-Art.md) · [Parallax](./Rendering/Parallax-Background.md)
10. [Health/HUD](./UI/Health-Bar-and-HUD.md) · [Menus and pause](./UI/Buttons-Menus-and-Pause.md) · [Audio](./Audio/Audio-Setup-and-Sound-Effects.md)
11. [Damage/knockback](./Gameplay/Damage-Knockback-and-Invulnerability.md) · [Pickups](./Gameplay/Pickups-and-Respawning.md) · [Checkpoints](./Gameplay/Checkpoints-and-Level-Progression.md) · [Save/load](./Gameplay/Save-Load-JSON.md)
12. [Projectiles](./Physics/Projectile-and-Damage.md) · [Melee hitboxes](./Physics/Melee-and-Area-Hitboxes.md) · [Enemy AI](./AI/Enemy-Patrol-and-Chase.md)
13. [Prefabs/data](./Core-Concepts/Prefabs-ScriptableObjects-and-Data.md) · [Time/coroutines](./Core-Concepts/Time-Coroutines-and-Invoke.md)
14. [Screen shake](./Recipes/Screen-Shake.md) · [Screen bounds/wrapping](./Recipes/Screen-Bounds-and-Wrapping.md)
15. [Debugging](./Debugging/Debugging-and-Profiling.md) · [Pooling/performance](./Optimization/Performance-and-Object-Pooling.md)

## Architecture reminder
Choose one movement authority per object. Keep gameplay rules separate from UI presentation, use layer masks intentionally, and validate references. Check [official 2D documentation](https://docs.unity3d.com/Manual/Unity2D.html) for version-specific behavior.
