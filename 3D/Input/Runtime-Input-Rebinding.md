# Input System: Runtime Key Rebinding

The Input System supports interactive rebinding for settings screens. Requirements: Input System package, an Input Actions asset, and a binding on the action to rebind. A composite binding such as a 2D Vector usually needs one of its parts selected (for example, Up), not the composite root.

## Rebinding component
Save as RuntimeRebind.cs. Assign the InputActionReference, exact binding index, InputActionAsset, and a unique PlayerPrefs key.

    using UnityEngine;
    using UnityEngine.InputSystem;

    public sealed class RuntimeRebind : MonoBehaviour
    {
        [SerializeField] private InputActionReference actionReference;
        [SerializeField] private InputActionAsset actionsAsset;
        [SerializeField, Min(0)] private int bindingIndex;
        [SerializeField] private string preferencesKey = "input-overrides";
        private InputActionRebindingExtensions.RebindingOperation operation;
        private bool actionWasEnabled;

        private void Awake() => LoadOverrides();

        public void StartRebind()
        {
            if (operation != null || actionReference == null ||
                actionReference.action == null) return;

            InputAction action = actionReference.action;
            if (bindingIndex >= action.bindings.Count) return;

            actionWasEnabled = action.enabled;
            action.Disable();

            operation = action.PerformInteractiveRebinding(bindingIndex)
                .OnComplete(FinishRebind)
                .OnCancel(FinishRebind);

            operation.Start();
        }

        private void FinishRebind(InputActionRebindingExtensions.RebindingOperation op)
        {
            InputAction action = actionReference != null ? actionReference.action : null;
            if (action != null)
            {
                if (actionWasEnabled) action.Enable();
                else action.Disable();
            }

            if (operation == op) operation = null;
            op.Dispose();
            SaveOverrides();
        }

        public void SaveOverrides()
        {
            if (actionsAsset == null) return;
            PlayerPrefs.SetString(preferencesKey, actionsAsset.SaveBindingOverridesAsJson());
            PlayerPrefs.Save();
        }

        public void LoadOverrides()
        {
            if (actionsAsset == null) return;
            if (PlayerPrefs.HasKey(preferencesKey))
                actionsAsset.LoadBindingOverridesFromJson(
                    PlayerPrefs.GetString(preferencesKey));
        }

        private void OnDisable()
        {
            // Cancel invokes FinishRebind through the OnCancel callback.
            if (operation != null) operation.Cancel();
        }
    }

The operation is disposed on completion/cancel. Saving overrides is optional; this example uses PlayerPrefs for a simple local preference. Load overrides before enabling action maps. Don't share ownership of enable/disable state between unrelated systems.

## UX details
Display a “Press a key…” prompt while rebinding, show the resulting friendly control name, handle Escape/cancel, and warn if two actions now share the same binding. Use the Input System's Rebinding UI sample for a more complete workflow.

## Common mistakes
- Binding index points to composite root instead of a part.
- Binding override is never saved or restored.
- The shared action is forcibly enabled/disabled by multiple scripts.
- Rebind is not cancelled when a menu closes.
- UI displays raw binding paths instead of readable control names.
