# 2D: Gameplay Systems

## Guides
- [Scene transitions and pause](./Scene-Transitions-and-Pause.md)
- [Win/lose state flow](./Win-Lose-and-Game-State.md)
- [Tilemaps, sprites, and collision](./Tilemap-Sprites-and-Collision.md)
- [Damage, knockback, invulnerability](./Damage-Knockback-and-Invulnerability.md)
- [Pickups and respawning](./Pickups-and-Respawning.md)
- [Checkpoints and level progression](./Checkpoints-and-Level-Progression.md)
- [Save/load JSON](./Save-Load-JSON.md)

## Recommended ownership
Gameplay systems own rules/state; UI displays those values. Health should not know HUD layout. Pickups award collection state once, then notify inventory/score. Checkpoints store a position while respawn logic decides how velocity, health, and items are restored.

## Testing checklist
Test repeated triggers, missing references, scene reload, timeScale restoration, checkpoint at world origin, and missing/malformed save files.
