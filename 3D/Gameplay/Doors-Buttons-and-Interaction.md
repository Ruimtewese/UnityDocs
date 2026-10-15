# 3D Doors, Buttons, and Interaction

This is a component-based interaction pattern. The raycast controller detects what the camera points at; the target implements an interactable contract.

## Interface

    public interface IInteractable
    {
        string Prompt { get; }
        void Interact(GameObject interactor);
    }

Save this as IInteractable.cs. Add a component to the door that implements it.

    using UnityEngine;

    public sealed class SimpleDoor : MonoBehaviour, IInteractable
    {
        [SerializeField] private Transform hinge;
        [SerializeField] private Vector3 openLocalEuler = new Vector3(0f, 90f, 0f);
        [SerializeField] private Vector3 closedLocalEuler;
        private bool isOpen;

        public string Prompt => isOpen ? "Close door" : "Open door";

        public void Interact(GameObject interactor)
        {
            if (hinge == null) return;
            isOpen = !isOpen;
            hinge.localRotation = Quaternion.Euler(isOpen ? openLocalEuler : closedLocalEuler);
        }
    }

Create an empty hinge pivot at the door's hinge edge, parent the door mesh under it, and assign Hinge. Ensure the local Euler values suit the model's orientation. This immediate rotation is a prototype; use a Coroutine, Animator, or tweening package for an animated door.

## Raycast detector
A camera script can raycast from its center, get the hit component (or a parent component), then call Interact when the input action is triggered. Configure a range and layer mask; don't let UI prompts independently perform the interaction.

## Common mistakes
- Pivot is at the center instead of the hinge edge.
- Mesh/model local axes differ from assumed opening direction.
- Interface implementation is on a parent but only GetComponent is used.
- Target moves out of range between prompt and input.
