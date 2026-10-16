# 3D Unity Reference

Focused guides for Unity 6 3D games. Confirm API and package versions for the installed Editor. 3D uses Rigidbody, Collider, Physics, and NavMesh APIs rather than 2D counterparts.

## Recommended path
1. [Common Unity APIs](./Core-Concepts/Common-Unity-APIs.md) · [Transforms/math](./Core-Concepts/Transforms-and-Math.md) · [Vector math](./Core-Concepts/Vector-Math-Reference.md) · [Events/interfaces](./Core-Concepts/Events-and-Interfaces.md)
2. [Input System](./Input/Input-System-Setup.md) · [Cursor/sensitivity](./Input/Cursor-Lock-and-Mouse-Sensitivity.md) · [Runtime rebinding](./Input/Runtime-Input-Rebinding.md)
3. [CharacterController](./Movement/CharacterController-Guide.md) · [Slopes/steps](./Movement/CharacterController-Slopes-and-Steps.md) · [Jumping](./Movement/Jumping-and-Ground-Checks.md)
4. [Rigidbody movement](./Movement/Rigidbody-Movement.md) · [Camera-relative movement](./Movement/Camera-Relative-Rigidbody-Movement.md) · [Rigidbody jump](./Movement/Rigidbody-Jump-and-Ground-Check.md) · [Forces](./Physics/Forces-and-Rigidbody-Movement.md)
5. [Component Inspector reference](./Components/Inspector-Reference.md) · [Component overview](./Components/Component-Reference.md)
6. [Collision/layers](./Physics/Collision-Layers-and-Rigidbody.md) · [Raycasts](./Physics/Raycast-Interaction.md) · [Query reference](./Physics/Raycast-Spherecast-Overlap-Reference.md) · [Joints](./Physics/Joints-and-Constraints.md)
7. [First-/third-person camera](./Camera/First-and-Third-Person-Camera.md) · [Camera collision](./Camera/Camera-Collision-and-Smoothing.md)
8. [HUD/world UI](./UI/HUD-and-World-Space-UI.md) · [Settings/audio](./UI/Settings-Menu-and-Audio-Volume.md) · [UI Toolkit vs uGUI](./UI/UI-Toolkit-vs-uGUI.md) · [Spatial audio](./Audio/3D-Spatial-Audio.md)
9. [Animator parameters](./Animation/Animator-Parameters-and-Transitions.md) · [Blend Trees](./Animation/Blend-Trees-and-Locomotion.md) · [Animation Events](./Animation/Animation-Events.md) · [Root motion](./Animation/Root-Motion-vs-Code-Movement.md)
10. [Lighting/URP](./Rendering/Lighting-Materials-and-URP.md) · [Post-processing](./Rendering/Post-Processing-and-Camera.md) · [Particles/trails](./Rendering/Particle-Systems-and-Trails.md) · [Model import](./Modeling-and-Materials/Model-Import-Settings.md)
11. [Health/death](./Gameplay/Health-Damage-and-Death.md) · [Pickups/inventory](./Gameplay/Pickups-and-Interactables.md) · [Item definitions](./Gameplay/Inventory-and-Items.md)
12. [Doors/interactables](./Gameplay/Doors-Buttons-and-Interaction.md) · [Raycast interaction controller](./Gameplay/Interactable-Raycast-Controller.md) · [Trigger zones](./Gameplay/Trigger-Zones-and-Area-Events.md)
13. [Object pooling](./Gameplay/Object-Pooling.md) · [Prefab spawners](./Gameplay/Prefabs-and-Spawn-Systems.md) · [Hitscan/query examples](./Physics/Projectile-and-Hitscan-Weapons.md)
14. [NavMesh/state machines](./AI/State-Machines-and-NavMesh.md) · [Patrol/detection](./AI/Patrol-and-Detection.md) · [Line of sight](./AI/Line-of-Sight-and-Aggro.md)
15. [Scene flow/pause](./Gameplay/Game-Architecture-and-Scene-Flow.md) · [Async scene loading](./Gameplay/Scene-Loading-Async.md) · [Saving JSON](./Gameplay/Saving-Loading-JSON.md) · [Win/lose state](./Gameplay/Win-Lose-and-Game-State.md)
16. [Networking overview](./Networking/Multiplayer-Architecture-Overview.md) · [Localization (shared guide)](../2D/Core-Concepts/Localization-Setup.md)
17. [Addressables/resources](./Optimization/Addressables-and-Resources.md) · [Memory/loading](./Optimization/Memory-and-Asset-Loading.md) · [Version control](./Optimization/Version-Control-for-Unity.md) · [Testing](./Optimization/Unity-Test-Framework-Quickstart.md) · [Build/performance](./Optimization/Build-and-Performance-Checklist.md) · [Debugging](./Debugging/Debugging-and-Profiling.md)

## Expanded core systems (complete examples)
- [Combat and projectiles](./Systems/Combat-and-Projectiles.md) — damage contract, health, melee, projectile, launcher.
- [Enemy AI and combat states](./Systems/Enemy-AI-and-Combat-States.md) — patrol/chase/attack/return state machine.
- [Inventory and ScriptableObjects](./Systems/Inventory-and-ScriptableObjects.md) — item definitions, stacking inventory, pickups, save guidance.
- [Scene flow and save/load](./Systems/Scene-Flow-and-Save-Load.md) — async loading, pause controller, JSON save data.
- [UI, audio, and architecture](./Systems/UI-Audio-and-Architecture.md) — audio service, event-driven health text, reusable design rules.

Official references: [Unity Manual](https://docs.unity3d.com/Manual/index.html) · [Scripting API](https://docs.unity3d.com/ScriptReference/)
