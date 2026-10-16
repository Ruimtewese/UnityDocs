# 3D Inventory and ScriptableObject Items

Use ScriptableObjects for authored item definitions and component-owned runtime quantities.

## ItemDefinition.cs

~~~csharp
using UnityEngine;

[CreateAssetMenu(menuName = "Game/Items/Item Definition")]
public class ItemDefinition : ScriptableObject
{
    [SerializeField] private string itemId;
    [SerializeField] private string displayName;
    [SerializeField, TextArea] private string description;
    [SerializeField, Min(1)] private int maxStack = 99;
    [SerializeField] private Sprite icon;
    public string Id => itemId;
    public string DisplayName => displayName;
    public string Description => description;
    public int MaxStack => maxStack;
    public Sprite Icon => icon;
}
~~~

Create an asset using **Create → Game → Items → Item Definition** and assign a unique stable ID.

## Inventory3D.cs

~~~csharp
using System;
using System.Collections.Generic;
using UnityEngine;

[Serializable]
public class InventoryStack
{
    public ItemDefinition item;
    [Min(1)] public int quantity = 1;
}

public class Inventory3D : MonoBehaviour
{
    [SerializeField, Min(1)] private int capacity = 16;
    [SerializeField] private List<InventoryStack> slots = new();
    public IReadOnlyList<InventoryStack> Slots => slots;
    public event Action Changed;

    public bool Add(ItemDefinition item, int amount = 1)
    {
        if (!item || amount <= 0) return false;
        int left = amount;
        foreach (var stack in slots)
        {
            if (stack.item != item || stack.quantity >= item.MaxStack) continue;
            int n = Mathf.Min(left, item.MaxStack - stack.quantity);
            stack.quantity += n; left -= n;
            if (left == 0) { Changed?.Invoke(); return true; }
        }
        while (left > 0 && slots.Count < capacity)
        {
            int n = Mathf.Min(left, item.MaxStack);
            slots.Add(new InventoryStack { item = item, quantity = n });
            left -= n;
        }
        Changed?.Invoke();
        return left == 0;
    }

    public int Count(ItemDefinition item)
    {
        int total = 0;
        foreach (var stack in slots) if (stack.item == item) total += stack.quantity;
        return total;
    }

    public bool Remove(ItemDefinition item, int amount = 1)
    {
        if (!item || amount <= 0 || Count(item) < amount) return false;
        int left = amount;
        for (int i = slots.Count - 1; i >= 0 && left > 0; i--)
        {
            if (slots[i].item != item) continue;
            int n = Mathf.Min(left, slots[i].quantity);
            slots[i].quantity -= n; left -= n;
            if (slots[i].quantity == 0) slots.RemoveAt(i);
        }
        Changed?.Invoke();
        return true;
    }
}
~~~

## ItemPickup3D.cs

~~~csharp
using UnityEngine;

[RequireComponent(typeof(Collider))]
public class ItemPickup3D : MonoBehaviour
{
    [SerializeField] private ItemDefinition item;
    [SerializeField, Min(1)] private int amount = 1;
    private void Reset() => GetComponent<Collider>().isTrigger = true;

    private void OnTriggerEnter(Collider other)
    {
        var inventory = other.GetComponentInParent<Inventory3D>();
        if (inventory && item && inventory.Add(item, amount)) Destroy(gameObject);
    }
}
~~~

### Setup and design rules
Player: Inventory3D and a collider. Pickup: trigger Collider and ItemPickup3D with an item asset assigned. Runtime quantities must not be stored in the ScriptableObject because all actors share that asset. Save stable item IDs and quantities, then resolve IDs back to assets on load. This sample may partially add items before returning false if capacity is exhausted.
