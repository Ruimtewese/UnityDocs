# 3D Inventory and Item Definitions

Separate shared item definition data from each player's runtime inventory count. ScriptableObjects are useful for authored definitions; a runtime inventory is ordinary mutable state.

## Item definition
Save as ItemDefinition.cs.

    using UnityEngine;

    [CreateAssetMenu(menuName = "Game/Item Definition")]
    public sealed class ItemDefinition : ScriptableObject
    {
        public string itemId = "item.basic";
        public string displayName = "Basic Item";
        public Sprite icon;
        [TextArea] public string description;
        [Min(1)] public int maxStack = 99;
    }

Create definitions through Assets → Create → Game → Item Definition. Keep itemId unique and stable if inventory data will be saved.

## Runtime inventory
This minimal version supports a count per item definition; it does not yet support slots, weight, equipment, or serialization.

    using System.Collections.Generic;
    using UnityEngine;

    public sealed class SimpleInventory : MonoBehaviour
    {
        private readonly Dictionary<ItemDefinition, int> counts = new();

        public int GetCount(ItemDefinition item)
        {
            return item != null && counts.TryGetValue(item, out int count) ? count : 0;
        }

        public bool Add(ItemDefinition item, int amount)
        {
            if (item == null || amount <= 0) return false;
            int newCount = GetCount(item) + amount;
            counts[item] = Mathf.Min(newCount, item.maxStack);
            return true;
        }

        public bool Remove(ItemDefinition item, int amount)
        {
            if (item == null || amount <= 0 || GetCount(item) < amount) return false;
            int remaining = GetCount(item) - amount;
            if (remaining == 0) counts.Remove(item);
            else counts[item] = remaining;
            return true;
        }
    }

The Add method caps at maxStack but doesn't report any overflow; a full inventory system should return the quantity accepted and leave overflow in the world. Dictionaries are not directly serialized by JsonUtility; save a serializable list of item IDs/counts and resolve IDs to definitions on load.

## Common mistakes
- Item IDs change and old save data no longer resolves.
- ScriptableObject asset is mutated as if it were player-owned state.
- Inventory UI is treated as the source of truth.
- Add silently loses overflow when stacks are full.
