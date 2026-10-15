# UnityDocs — Unity 6 C# Reference

A practical, growing reference for 2D and 3D game development in Unity 6. Guides explain concepts, Inspector setup, dependencies, complete examples, common mistakes, and testing.

**Scope:** Unity is a large engine with changing packages and platform APIs. This handbook aims for broad coverage but cannot guarantee every API or third-party package is documented. Confirm unfamiliar APIs against the official docs for your installed Unity version.

## Start here
- [2D reference](./2D/README.md)
- [3D reference](./3D/README.md)

## Suggested learning path
1. Learn GameObjects, Components, Inspector, Scenes, and Prefabs.
2. Learn C# types, methods, classes, collections, and events.
3. Study lifecycle, input, movement, and physics.
4. Add camera, animation, UI, and audio.
5. Add scene flow, saving, data assets, and AI.
6. Debug and profile a Development Build on target hardware.

## Reference conventions
- Baseline: Unity 6; API and package differences may exist across versions.
- Input System examples require the package and configured actions unless noted.
- Rigidbody motion belongs in the physics step or a suitable Rigidbody API; avoid conflicting Transform writes.
- A script is copy-ready only when its required components, dependencies, Inspector references, and input setup are stated.
- MonoBehaviour class names must match their C# filenames.
- Unity 6 Rigidbody examples use linearVelocity; older versions may use velocity.
- Use relative links so pages work on GitHub and local clones.

## Official references
- [Unity Manual](https://docs.unity3d.com/Manual/index.html)
- [Scripting API](https://docs.unity3d.com/ScriptReference/)
- [Unity Learn](https://learn.unity.com/)
- [Input System manual](https://docs.unity3d.com/Packages/com.unity.inputsystem@latest)
- [2D features](https://docs.unity3d.com/Manual/Unity2D.html)
- [Physics](https://docs.unity3d.com/Manual/PhysicsSection.html)

## Main topics
Components and Inspector settings; C# lifecycle and architecture; input; movement and math; physics/collision; sprites, tilemaps, animation; prefabs and ScriptableObjects; camera/rendering; UI/audio; gameplay state; AI; save/load; debugging; performance; and build checks.

The handbook is expanded topic by topic. Prefer focused guides with prerequisites, exact setup, complete examples, edge cases, and authoritative links.
