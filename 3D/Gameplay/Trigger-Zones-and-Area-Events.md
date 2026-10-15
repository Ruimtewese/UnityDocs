# 3D Trigger Zones and Area Events

Trigger volumes detect objects entering, staying inside, and leaving a region. They are useful for checkpoints, doors, tutorial prompts, damage zones, and level boundaries.

## Zone component
Create a BoxCollider or SphereCollider and enable Is Trigger. Ensure the collider/Rigidbody combination satisfies Unity's trigger requirements. Assign the tag that should activate the zone.

    using UnityEngine;

    public sealed class TriggerZone3D : MonoBehaviour
    {
        [SerializeField] private string requiredTag = "Player";
        [SerializeField] private GameObject prompt;
        private bool occupied;

        private void Awake()
        {
            if (prompt != null) prompt.SetActive(false);
        }

        private void OnTriggerEnter(Collider other)
        {
            if (occupied || !other.CompareTag(requiredTag)) return;
            occupied = true;
            if (prompt != null) prompt.SetActive(true);
        }

        private void OnTriggerExit(Collider other)
        {
            if (!other.CompareTag(requiredTag)) return;
            occupied = false;
            if (prompt != null) prompt.SetActive(false);
        }
    }

This simple boolean assumes only one matching actor enters at a time. For multiplayer or multiple same-tag actors, track each entrant (for example, with a HashSet) instead of a single occupied flag. Ensure child colliders don't cause enter/exit flicker as a character moves.

## Physics and filtering
Use layers to limit which objects can trigger the zone. A trigger callback is distinct from a raycast/overlap query. For a long-lived volume, add logic for actors that are disabled/destroyed while inside so state can be cleared safely.

## Common mistakes
- Is Trigger is off.
- Rigidbody setup does not qualify for trigger callbacks.
- Required tag isn't assigned.
- Several player colliders produce repeated enters/exits.
- Prompt stays active when the actor is destroyed or disabled in the zone.
