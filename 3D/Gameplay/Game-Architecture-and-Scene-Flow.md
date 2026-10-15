# Game Architecture and Scene Flow

## A practical small-game structure
- Input reader translates devices into intent.
- Controller converts intent into movement.
- Health/damage owns health and death events.
- UI presenter displays state without owning gameplay rules.
- Game-flow manager handles pause, game over, and level changes.
- ScriptableObjects store shared authored definitions.
- Save system persists progress separately from scene objects.

Do not create a class for every tiny feature. Choose the simplest structure that keeps ownership clear.

## Scene flow
Use SceneManager and include scenes in the active Build Profile. If a manager uses DontDestroyOnLoad, prevent duplicates and plan cleanup. Do not let several scripts independently own global time scale, audio volume, or scene transitions.

## Dependencies
Use serialized references for scene-local dependencies. Use events for notifications. Use interfaces when different concrete object types share behavior. Global singletons can be convenient but hide dependencies and complicate tests.

    public interface IDamageable
    {
        void TakeDamage(float amount);
    }

A weapon can call TakeDamage on any object implementing IDamageable. Interfaces are ordinary C#, but serializing interface references in the Inspector requires a wrapper or a serialized Unity object reference with a cast.

## Checklist
- Every state has a clear owner.
- Event subscribers unsubscribe when disabled or destroyed.
- Scene transitions restore pause/time state.
- Authored definitions are separate from mutable save data.
- Missing references produce clear errors.
- UI is not the only place where gameplay rules exist.
