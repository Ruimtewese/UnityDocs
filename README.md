# UnityDocs — The Practical Unity Reference

A structured, growing reference for building games with Unity. Topics are split into **2D** and **3D** workflows, each organized into focused subfolders.

> **Version note:** Inspector labels, APIs, and packages can differ by Unity version and render pipeline. Verify version-sensitive details against the [Unity Manual](https://docs.unity.com/en-us) and [Scripting API](https://docs.unity3d.com/ScriptReference/).

## Start here
- [2D Development](./2D/README.md) — sprites, orthographic cameras, Rigidbody2D, Tilemaps, sorting, and 2D animation.
- [3D Development](./3D/README.md) — meshes, materials, Rigidbody, colliders, character control, lighting, and rendering.
- [Unity Manual](https://docs.unity.com/en-us)
- [Unity Scripting API](https://docs.unity3d.com/ScriptReference/)
- [Unity Learn](https://learn.unity.com/)

## Structure
```text
UnityDocs/
├── 2D/
│   ├── Core-Concepts/  Components/  Movement/  Physics/  Input/
│   ├── Animation/  Cameras/  Tilemaps/  UI/  Audio/
│   ├── Rendering/  Optimization/  Recipes/
└── 3D/
    ├── Core-Concepts/  Components/  Movement/  Physics/  Input/
    ├── Animation/  Cameras/  Modeling-and-Materials/  UI/  Audio/
    ├── Lighting-and-Rendering/  Optimization/  Recipes/
```
Each topic folder contains a README so GitHub displays the folder and its starting guide.

## How to use this reference
1. Read the domain overview, then open the topic that matches your task.
2. Check the required components, Inspector settings, layers, tags, and packages before using code.
3. Test examples in a small scene and inspect the Console when something fails.
4. Profile representative builds before optimizing.

## Core Unity concepts
- **GameObject:** a scene object that holds components.
- **Component:** provides data or behaviour; Transform is present on every GameObject.
- **Transform:** position, rotation, and scale in local or world space.
- **Prefab:** reusable GameObject setup that can be instantiated.
- **Scene:** a saved collection of GameObjects and scene settings.
- **MonoBehaviour:** base class for scripts attached to GameObjects and eligible for Unity lifecycle callbacks.
- **Package:** optional or separately versioned functionality managed through Package Manager.

## Documentation standards
Topic guides should explain what a system does, when to use it, key Inspector fields and units, relevant API methods/callbacks, complete examples and setup, common errors, performance implications, and official references. State the tested Unity version and required packages whenever possible.

## Important habits
- Keep input, game state, physics, presentation, and UI responsibilities understandable.
- Prefer serialized references over repeated scene-wide searches.
- Use physics APIs consistently for physics-driven objects.
- Do not assume a setting's default is identical across every Unity version.
- Optimize from profiler evidence, not guesswork.
