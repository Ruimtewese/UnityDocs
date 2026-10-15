# 3D: Gameplay Systems

## Guides
- [Game architecture and scene flow](./Game-Architecture-and-Scene-Flow.md)
- [Win/lose and game state](./Win-Lose-and-Game-State.md)
- [Scene transitions and pause](./Scene-Transitions-and-Pause.md)
- [Asynchronous scene loading](./Scene-Loading-Async.md)
- [Health, damage, and death](./Health-Damage-and-Death.md)
- [Pickups and interactables](./Pickups-and-Interactables.md)
- [Doors, buttons, and interaction](./Doors-Buttons-and-Interaction.md)
- [Camera raycast interaction](./Interactable-Raycast-Controller.md)
- [Trigger zones and area events](./Trigger-Zones-and-Area-Events.md)
- [Inventory and item definitions](./Inventory-and-Items.md)
- [Prefab spawning](./Prefabs-and-Spawn-Systems.md)
- [Object pooling](./Object-Pooling.md)
- [Saving/loading JSON](./Saving-Loading-JSON.md)
- [Cooldowns and temporary effects](./Cooldowns-Timers-and-Temporary-Effects.md)

## Recommended ownership
Keep gameplay state separate from presentation. Health owns health values; game flow determines death behavior; UI reads state; inventory owns runtime item counts; ScriptableObjects hold item definitions; save data stores stable IDs/counts instead of scene object references.

## Testing checklist
Test repeated callbacks, missing references, pause/resume and scene transitions, invalid save data, full inventory stacks/overflow, and objects destroyed while prompts still refer to them.
