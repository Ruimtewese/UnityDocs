# Tags, GameObject Layers, Sorting Layers, and LayerMasks

These systems have related names but different purposes. Choose the one matching the problem.

## Tags
Tags identify a GameObject with one label, such as Player, Enemy, or Checkpoint. Use CompareTag for comparisons. Tags are not a collision filter; they answer identity questions.

    if (other.CompareTag("Player"))
    {
        // Handle a player entering a trigger.
    }

The tag must exist and be assigned exactly as the code expects.

## GameObject Layers
Layers group objects for cameras, physics filtering, and raycasts. A Camera Culling Mask decides which layers it renders. The Physics 2D or 3D collision matrix controls which layer pairs can interact.

## LayerMask
LayerMask stores a set of selected layers as bits. In the Inspector, choose layer names through the mask dropdown. In code, LayerMask.GetMask("Ground", "Enemy") is safer than manually typing bit values. Ensure names exist and match capitalization.

## Sorting Layers
Sorting Layers and Order in Layer control 2D renderer draw order. They do not determine physics collision. Changing a GameObject's Layer will not fix SpriteRenderer order, and changing its Sorting Layer will not stop a collider from colliding.

## Example: raycast filter

    using UnityEngine;

    public sealed class LayerFilteredRay2D : MonoBehaviour
    {
        [SerializeField] private LayerMask targetLayers;
        [SerializeField, Min(0f)] private float distance = 5f;

        public bool HasTarget()
        {
            RaycastHit2D hit = Physics2D.Raycast(
                transform.position, transform.right, distance, targetLayers);
            return hit.collider != null;
        }
    }

## Troubleshooting sequence
1. Verify the object's tag/layer in Inspector.
2. Check camera culling mask when objects do not render.
3. Check Layer Collision Matrix when physics callbacks do not fire.
4. Check the query's LayerMask when raycasts/overlaps miss.
5. Check Sorting Layer and Order in Layer when 2D sprites render behind the wrong object.
