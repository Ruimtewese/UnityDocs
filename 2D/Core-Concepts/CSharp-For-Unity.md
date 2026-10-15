# C# Essentials for Unity Scripts

This is a Unity-focused reminder, not a complete C# language specification.

## Common types
- int: whole numbers and counters.
- float: decimal values used by most Unity math.
- bool: true/false state.
- string: text.
- Vector2 / Vector3: coordinates or directions.
- Quaternion: rotation representation.
- GameObject: scene object; Component: behavior attached to an object.
- Transform: position, rotation, and scale.
- List<T>: growable ordered collection; Dictionary<TKey,TValue>: keyed lookup collection.

## Fields, properties, and methods

    [SerializeField] private float speed = 5f;
    public int Score { get; private set; }

    public void AddScore(int amount)
    {
        if (amount <= 0) return;
        Score += amount;
    }

SerializeField makes a private field editable in the Inspector. A property controls code access, but Unity's default Inspector serialization does not serialize ordinary properties. Use fields for serialized data and properties for runtime access when appropriate.

## Branching and loops

    if (health <= 0f)
    {
        // Handle death.
    }
    else
    {
        // Continue playing.
    }

    for (int i = 0; i < items.Count; i++)
    {
        Debug.Log(items[i]);
    }

Use return to exit a method early, break to exit a loop, and continue to skip to the next loop iteration. Check collection length before indexing.

## Null and reference safety
A Unity reference may be missing, destroyed, or not assigned. Check references before use:

    if (target == null)
    {
        Debug.LogWarning("Target is missing.", this);
        return;
    }

NullReferenceException means code accessed a member through a missing reference. Trace where it should have been assigned or created.

## Common misconceptions
- = assigns; == compares.
- && requires both conditions; || needs either condition.
- Integer division can discard the fractional part; use floating-point operands when needed.
- Arrays have fixed length; List<T> can grow.
- public does not guarantee that Unity serializes a field if its type/shape is unsupported.
- MonoBehaviour scripts normally attach to GameObjects; ScriptableObjects are asset-style data objects.
- Standard MonoBehaviour class names should match their C# filenames.
