# 2D: Gameplay Systems

## Guides
- [Scene transitions and pause](./Scene-Transitions-and-Pause.md)
- [Tilemaps, sprites, and collision](./Tilemap-Sprites-and-Collision.md)
- [Damage, knockback, invulnerability](./Damage-Knockback-and-Invulnerability.md)
- [Pickups and respawning](./Pickups-and-Respawning.md)
- [Checkpoints and level progression](./Checkpoints-and-Level-Progression.md)
- [Save/load JSON](./Save-Load-JSON.md)

## Recommended ownership
Gameplay systems should own rules and state; UI displays those values. Health should not need to know how the HUD is laid out. A pickup should award collection state once, then notify inventory/score. A checkpoint should store a position while respawn logic decides how velocity, health, and inventory are restored.

## Testing checklist
Test repeated triggers, invalid/missing references, scene reload, pause state restoration, checkpoint respawn at world origin, and saving when a file is missing or malformed.
