# 3D Inventory and Item Definitions

Separate shared authored item definitions from each player's runtime inventory. ScriptableObjects are useful for definitions; inventory counts are mutable runtime state.

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

Create definitions through the Project window's Create menu. Keep itemId unique and stable if inventory data will be saved.

## Runtime inventory
This minimal example supports one count per item definition; it does not implement slots, weight, equipment, or serialization. Add returns the number of items accepted so callers can keep any overflow.

    using System.Collections.Generic;
    using UnityEngine;

    public sealed class SimpleInventory : MonoBehaviour
    {
        private readonly Dictionary<ItemDefinition, int> counts = new();

        public int GetCount(ItemDefinition item)
        {
            return item != null && counts.TryGetValue(item, out int count) ? count : 0;
        }

        public int Add(ItemDefinition item, int amount)
        {
            if (item == null || amount <= 0) return 0;
            int current = GetCount(item);
            int accepted = Mathf.Min(amount, Mathf.Max(0, item.maxStack - current));
            if (accepted > 0) counts[item] = current + accepted;
            return accepted;
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

If Add returns less than requested amount, keep or drop the overflow according to game rules. Dictionaries are not directly serialized by JsonUtility; save a serializable list of item IDs/counts and resolve IDs to definitions during load.

## Common mistakes
- Item IDs change, so existing saves no longer resolve.
- ScriptableObject assets are mutated as player-owned state.
- Inventory UI becomes the source of truth instead of displaying inventory data.
- Overflow is silently discarded.
