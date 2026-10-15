# 3D Pickups and Interactable Objects

## Trigger pickup
Create a prefab with a visible mesh, Collider set to Is Trigger, and this script. The player needs the tag configured in the Inspector.

    using UnityEngine;

    public sealed class Pickup3D : MonoBehaviour
    {
        [SerializeField] private string playerTag = "Player";
        [SerializeField] private int value = 1;
        private bool collected;

        private void OnTriggerEnter(Collider other)
        {
            if (collected || !other.CompareTag(playerTag)) return;
            collected = true;

            PlayerInventory inventory = other.GetComponentInParent<PlayerInventory>();
            if (inventory != null) inventory.AddCoins(value);
            Destroy(gameObject);
        }
    }

Minimal inventory example:

    using UnityEngine;

    public sealed class PlayerInventory : MonoBehaviour
    {
        public int Coins { get; private set; }

        public void AddCoins(int amount)
        {
            if (amount > 0) Coins += amount;
        }
    }

A valid Rigidbody/collider relationship is needed for trigger callbacks. Consider using an explicit interface/event to support multiple actor types without relying on a specific tag.

## Raycast interaction
For doors, switches, and terminals, cast from the center of the camera. If a ray hits an object implementing an interaction component, call its Interact method. Limit range and use an interactable layer mask. Keep interaction logic separate from the UI prompt that says “Press E”.

## Safety for interactions
Validate the target again when the action occurs; do not trust only a stale UI prompt. Clear focus when the object leaves range or is destroyed. Avoid letting a prompt's hidden UI object receive gameplay input accidentally.

## Common mistakes
- Collider isn't a trigger or the Rigidbody setup is invalid.
- Player tag differs from configured tag.
- Inventory reference is on the player root but lookup happens on a child.
- A pickup awards multiple times because there is no collected guard.
