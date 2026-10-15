# Unity Multiplayer Architecture Overview

Multiplayer is an architecture choice, not merely a component added to a single-player game. Decide authority, synchronization, joining/leaving, latency behavior, and cheating threat model before building many game systems on local-only assumptions.

## Pick a networking model
- **Netcode for GameObjects (NGO):** high-level netcode for GameObject/MonoBehaviour projects. Usually the easier Unity-native path when your project is organized around GameObjects.
- **Netcode for Entities:** a data-oriented networking stack for Entities/DOTS architectures, aimed at projects needing that data-oriented model and advanced optimization.
- **Custom networking:** appropriate only when you understand transport, serialization, synchronization, reliability, and security requirements. It is not automatically simpler than an existing package.

Official Unity 6 references:
- [Multiplayer overview](https://docs.unity3d.com/6000.0/Documentation/Manual/multiplayer-overview.html)
- [Netcode for GameObjects](https://docs.unity3d.com/6000.0/Documentation/Manual/com.unity.netcode.gameobjects.html)
- [Netcode for Entities](https://docs.unity3d.com/6000.0/Documentation/Manual/com.unity.netcode.html)

## Server/host authority
A client sends an intent or request; the authority checks whether it is valid and decides the authoritative result. Avoid trusting a client to announce important outcomes such as currency, damage, score, or match victory. Even peer-hosted games need clear ownership rules.

## Replication basics
You must decide which state is authoritative and which state is replicated:
- Player input/intent
- Character position/rotation
- Health and other gameplay values
- Interactable/object state
- Match state and timers
- Spawn/despawn and scene/level state

Replicating every field every frame is rarely the right default. Use the network package's supported NetworkVariable/RPC or equivalent mechanism, and understand ownership/authority rules for your installed version.

## Sessions versus gameplay synchronization
A lobby/session system handles steps such as creating/joining a group of players and establishing a connection. The netcode layer handles gameplay replication. A session does not automatically sync player movement, health, or game state.

## Latency and testing
- Test at least two independent clients, not only one Editor instance.
- Simulate latency and packet loss when the tooling supports it.
- Don't assume commands arrive instantly or only once.
- Handle disconnects, reconnects, scene changes, player ownership, and host departure.
- Test with authoritative logic on the host/server and clients that send requests.
- Avoid synchronizing local UI state when it can be derived from authoritative gameplay state.

## Security and fairness
Never put private server credentials in a client build. Client builds can be inspected and modified. Validate competitive results on the trusted authority; local save files and client-side checks are not security boundaries.

## Suggested build order
1. Start with one networked object and a local host/client session.
2. Confirm spawning, ownership, and despawning.
3. Synchronize one small gameplay value.
4. Add player movement and test authority/interpolation.
5. Add game state, late joins, disconnect handling, and scene transitions.
6. Profile bandwidth and test poor network conditions.
