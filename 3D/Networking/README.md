# Unity Networking and Multiplayer

This folder covers multiplayer architecture in Unity. Networked games require package configuration, testing with multiple clients, authority rules, and latency handling—not just synchronizing a Transform.

## Guide
- [Multiplayer architecture overview](./Multiplayer-Architecture-Overview.md)

## Current Unity paths
- [Netcode for GameObjects](https://docs.unity3d.com/6000.0/Documentation/Manual/com.unity.netcode.gameobjects.html): high-level networking for GameObject/MonoBehaviour workflows.
- [Netcode for Entities](https://docs.unity3d.com/6000.0/Documentation/Manual/com.unity.netcode.html): DOTS/Entities-oriented multiplayer.
- [Unity multiplayer overview](https://docs.unity3d.com/6000.0/Documentation/Manual/multiplayer-overview.html)

Choose the framework based on project architecture, package version, platform support, and synchronization needs. Avoid treating a lobby/session package as the system that automatically replicates gameplay state.
