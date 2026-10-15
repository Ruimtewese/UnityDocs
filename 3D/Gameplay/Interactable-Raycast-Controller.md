# 3D Camera Raycast Interaction Controller

This generic interaction controller looks from the center of the camera, finds an interactable component, and calls Interact when a button is pressed. The target script can be a door, switch, terminal, pickup, or other interaction object.

## Interface
Save as IInteractable.cs.

    using UnityEngine;

    public interface IInteractable
    {
        string Prompt { get; }
        void Interact(GameObject interactor);
    }

## Controller
Save as CameraInteractor3D.cs. Requires the Input System package, Camera reference, and an Interact action set up as a Button. Connect the action callback or call TryInteract from your input layer.

    using UnityEngine;

    public sealed class CameraInteractor3D : MonoBehaviour
    {
        [SerializeField] private Camera viewCamera;
        [SerializeField, Min(0.1f)] private float range = 3f;
        [SerializeField] private LayerMask layers = ~0;

        public string CurrentPrompt { get; private set; }

        private void Update()
        {
            CurrentPrompt = string.Empty;
            if (viewCamera == null) return;

            Ray ray = viewCamera.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0f));
            if (!Physics.Raycast(ray, out RaycastHit hit, range, layers,
                    QueryTriggerInteraction.Ignore))
                return;

            IInteractable interactable = FindInteractable(hit.collider);
            if (interactable != null) CurrentPrompt = interactable.Prompt;
        }

        public void TryInteract()
        {
            if (viewCamera == null) return;
            Ray ray = viewCamera.ViewportPointToRay(new Vector3(0.5f, 0.5f, 0f));
            if (!Physics.Raycast(ray, out RaycastHit hit, range, layers,
                    QueryTriggerInteraction.Ignore))
                return;

            IInteractable interactable = FindInteractable(hit.collider);
            interactable?.Interact(gameObject);
        }

        private static IInteractable FindInteractable(Collider hit)
        {
            MonoBehaviour[] behaviours = hit.GetComponentsInParent<MonoBehaviour>();
            foreach (MonoBehaviour behaviour in behaviours)
                if (behaviour is IInteractable item) return item;
            return null;
        }
    }

The controller reads the target every Update for a prompt, but rechecks at the moment of interaction so stale targets don't activate. If the interactor should be the player's root rather than the camera/controller object, assign and pass that root explicitly.

## Common mistakes
- Interface component is on a parent, so the code searches only the hit child.
- Target layer is not included in the mask.
- The prompt isn't cleared when nothing is hit.
- Input triggers TryInteract on both press and hold, repeating an action.
- A stale prompt is trusted without rechecking range.
