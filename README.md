# UnityDocs — Unity 6 C# Reference

A practical reference for building 2D and 3D games in Unity. The handbook focuses on complete workflows, setup instructions, copy-ready examples, common mistakes, and debugging checklists.

**Scope:** Unity is a large engine with evolving APIs and packages. This handbook is a broad, actively expanded reference, not a guarantee that every Unity API or third-party package is covered. Confirm version-sensitive details against the official documentation for your installed Editor/package versions.

## Start here

- [2D reference and learning path](./2D/README.md)
- [3D reference and learning path](./3D/README.md)

## Expanded practical guides

### 2D

- [Platformer controller](./2D/Movement/Platformer-Controller.md)
- [Collision layers and triggers](./2D/Physics/Collision-Layers-and-Triggers.md)
- [Health and damage system](./2D/Gameplay/Health-and-Damage-System.md)
- [Object pooling](./2D/Gameplay/Object-Pooling-2D.md)
- [Pixel-art import settings](./2D/Rendering/Pixel-Art-Import-Settings.md)

### 3D

- [Character controller](./3D/Movement/Character-Controller.md)
- [Mouse look](./3D/Movement/Mouse-Look.md)
- [Rigidbody physics](./3D/Physics/Rigidbody-Physics.md)
- [Collision matrix and triggers](./3D/Physics/Collision-Matrix-and-Triggers.md)
- [Health and damage system](./3D/Gameplay/Health-and-Damage-System.md)
- [JSON save system](./3D/Gameplay/Save-System-JSON.md)
- [LOD and occlusion culling](./3D/Rendering/LOD-and-Occlusion.md)
- [Networking architecture](./3D/Networking/Networking-Architecture.md)

## Suggested learning path

1. Learn GameObjects, Components, Inspector, Scenes, and Prefabs.
2. Learn core C#, Unity's lifecycle, and object/component APIs.
3. Set up input and choose movement/physics architecture.
4. Add camera, rendering, animation, UI, and audio.
5. Build health, pickups, inventory, AI, scene flow, and saving.
6. Test, debug, profile, and build for the target platform.

## Conventions

- Baseline: Unity 6, with API differences noted where known.
- Input examples identify when they use the legacy Input Manager; new projects can use the Input System package.
- Physics-driven motion should use the appropriate Rigidbody APIs and physics timing. Avoid scripts fighting over the same Transform/Rigidbody.
- Copy-ready scripts state class filename, required components, Inspector fields, and important dependencies.
- Use relative links so the handbook works on GitHub and in local clones.
- Treat snippets as starting points: integrate them with the project's existing architecture and test them in the Editor.

## Official documentation

- [Unity Manual](https://docs.unity3d.com/Manual/index.html)
- [Unity Scripting API](https://docs.unity3d.com/ScriptReference/)
- [Unity Learn](https://learn.unity.com/)
- [Input System manual](https://docs.unity3d.com/Packages/com.unity.inputsystem@latest)
- [2D feature manual](https://docs.unity3d.com/Manual/Unity2D.html)
- [Physics manual](https://docs.unity3d.com/Manual/PhysicsSection.html)
- [AI Navigation package](https://docs.unity3d.com/Packages/com.unity.ai.navigation@latest)

## Handbook maintenance

Keep guides focused on one workflow or concept. Include dependencies, exact setup, full examples, edge cases, common mistakes, testing, and official references. Prefer documented behavior over assumptions and test scripts in the Editor before relying on them in a project.
