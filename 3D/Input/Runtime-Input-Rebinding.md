# Input System: Runtime Key Rebinding

The Input System supports interactive rebinding for controls/settings screens. It requires an Input Action configured in an Input Actions asset and the Input System package. Rebinding should target a specific binding index; composite bindings often require rebinding their parts (for example, the Up part of a WASD composite).

## Rebinding component
Save as RuntimeRebind.cs and assign the target InputActionReference plus the binding index to rebind. Hook StartRebind from a settings button and display the resulting override path in UI as appropriate.

    using UnityEngine;
    using UnityEngine.InputSystem;

    public sealed class RuntimeRebind : MonoBehaviour
    {
        [SerializeField] private InputActionReference actionReference;
        [SerializeField, Min(0)] private int bindingIndex;
        private InputActionRebindingExtensions.RebindingOperation operation;

        public void StartRebind()
        {
            if (actionReference == null || actionReference.action == null) return;
            InputAction action = actionReference.action;
            if (bindingIndex >= action.bindings.Count) return;

            action.Disable();
            operation?.Dispose();
            operation = action.PerformInteractiveRebinding(bindingIndex)
                .OnComplete(op =>
                {
                    action.Enable();
                    op.Dispose();
                    operation = null;
                })
                .OnCancel(op =>
                {
                    action.Enable();
                    op.Dispose();
                    operation = null;
                });
            operation.Start();
        }

        private void OnDisable()
        {
            if (operation != null)
            {
                operation.Cancel();
                operation.Dispose();
                operation = null;
            }
        }
    }

After rebinding, persist overrides if the settings should survive application restarts. The asset's original binding is not overwritten in the project; the active action has an override. Handle cancel/duplicate controls and UI feedback in a production settings screen.

## Common mistakes
- Binding index points at the composite root instead of a specific composite part.
- A shared action is disabled while another system still depends on it.
- Rebind is not cancelled/disposed when the menu closes.
- Override isn't saved/restored across sessions.
- Rebinding callback reports raw paths rather than friendly control names.
