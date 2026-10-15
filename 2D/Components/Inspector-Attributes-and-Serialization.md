# Inspector Attributes and Serialization Reference

Unity serializes supported fields on MonoBehaviours and ScriptableObjects. Ordinary C# properties are not automatically shown or serialized by the default Inspector.

## Common attributes

    [SerializeField] private float speed = 5f;
    [Header("Movement")]
    [Tooltip("World units per second.")]
    [Min(0f)] [SerializeField] private float moveSpeed = 5f;
    [Range(0f, 1f)] [SerializeField] private float volume = 1f;
    [TextArea] [SerializeField] private string notes;
    [Space] [SerializeField] private Transform target;
    [HideInInspector] public int runtimeCounter;
    [System.NonSerialized] public float transientValue;

- SerializeField: serialize a supported private field and show it in Inspector.
- Header: section heading.
- Tooltip: hover description.
- Min: indicates a minimum for supported Inspector workflows; validate in code when correctness is important.
- Range: numeric slider.
- TextArea: multiline text field.
- Space: visual spacing.
- HideInInspector: hide a serialized field from Inspector.
- NonSerialized: exclude a field from Unity serialization.

## Component requirements and editor commands

    using UnityEngine;

    [RequireComponent(typeof(Rigidbody2D))]
    public sealed class NeedsBody : MonoBehaviour
    {
        [ContextMenu("Reset Runtime State")]
        private void ResetRuntimeState()
        {
            // Reset values for a convenient editor command.
        }
    }

RequireComponent helps ensure dependencies are present when the component is added, but runtime validation is still useful. ContextMenu exposes a method in the component Inspector menu. Avoid destructive actions that are easy to trigger accidentally.

## Renaming serialized fields
Use UnityEngine.Serialization.FormerlySerializedAs when renaming a field and preserving existing values matters.

    using UnityEngine;
    using UnityEngine.Serialization;

    public sealed class ExampleSettings : MonoBehaviour
    {
        [FormerlySerializedAs("oldSpeed")]
        [SerializeField] private float speed = 5f;
    }

## Serialization rules
- Unity's built-in serializer supports a specific set of fields and collection patterns.
- Dictionaries, arbitrary properties, and many nested generic types are not supported directly by the default serializer.
- Avoid static fields for Inspector configuration.
- Serialized object references may become missing if assets/objects are deleted or moved.
- After changing serialized field names/types, inspect existing prefabs/scenes for lost data.
