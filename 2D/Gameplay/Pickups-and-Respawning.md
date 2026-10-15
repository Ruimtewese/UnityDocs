# 2D Pickups and Respawning

## Pickup setup
Create a pickup GameObject with SpriteRenderer and Collider2D. Enable Is Trigger. Add a Rigidbody2D set to Kinematic if needed for the desired trigger setup. Put the player on a layer that the pickup detects.

## Collect once
Save as Pickup2D.cs.

    using UnityEngine;

    public sealed class Pickup2D : MonoBehaviour
    {
        [SerializeField, Min(0)] private int points = 1;
        [SerializeField] private string playerTag = "Player";
        [SerializeField] private GameObject visualRoot;
        private bool collected;

        private void OnTriggerEnter2D(Collider2D other)
        {
            if (collected || !other.CompareTag(playerTag)) return;
            collected = true;

            ScoreCounter score = FindFirstObjectByType<ScoreCounter>();
            if (score != null) score.Add(points);

            if (visualRoot != null) visualRoot.SetActive(false);
            Destroy(gameObject);
        }
    }

The example expects a ScoreCounter component; add the counter below. For a large game, prefer an explicit reference/event or player interface rather than searching the scene at collection time.

    using UnityEngine;
    public sealed class ScoreCounter : MonoBehaviour
    {
        public int Score { get; private set; }
        public void Add(int amount)
        {
            if (amount > 0) Score += amount;
        }
    }

Set the player tag to exactly Player in the Tag Manager and assign it to the player. Alternatively, use layers or a component/interface check. If you want a pickup to respawn, do not Destroy it; disable its visual/collider and start a coroutine to reactivate it after a delay.

## Respawn pattern
Keep the pickup GameObject active so its coroutine host survives; disable only the child visuals and collider. After collecting, wait, restore visuals/collider, and reset the collected flag. If the whole pickup GameObject is deactivated, its coroutine will not continue.

## Common mistakes
- Tag spelling/capitalization differs.
- Trigger collider has no valid physics pair.
- Pickup can award points multiple times due to missing collected guard.
- A coroutine host is deactivated when its timer needs to complete.
- Destroying the object prevents respawning that same instance.
