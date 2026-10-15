# 3D: Gameplay Systems

## Guides
- [Game architecture and scene flow](./Game-Architecture-and-Scene-Flow.md)
- [Scene transitions and pause](./Scene-Transitions-and-Pause.md)
- [Health, damage, and death](./Health-Damage-and-Death.md)
- [Pickups and interactables](./Pickups-and-Interactables.md)
- [Doors, buttons, and interaction](./Doors-Buttons-and-Interaction.md)
- [Inventory and item definitions](./Inventory-and-Items.md)
- [Prefab spawning](./Prefabs-and-Spawn-Systems.md)
- [Save/load JSON](./Saving-Loading-JSON.md)
- [Cooldowns and temporary effects](./Cooldowns-Timers-and-Temporary-Effects.md)

## Recommended ownership
Keep gameplay state separate from presentation. Health owns health values; game flow determines the death result; UI reads state; inventory owns runtime item counts; ScriptableObjects hold item definitions; save data stores stable IDs/counts rather than scene object references.

## Testing checklist
Test repeated collision callbacks, missing prefab/references, pause/resume and scene transitions, invalid save data, full inventory stacks/overflow, and objects that are destroyed while UI prompts still refer to them.
