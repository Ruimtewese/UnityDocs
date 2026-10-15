# 2D Ground Checks and Layer Masks

Ground checks decide whether a platformer may jump. A single ray from the player's center can miss edges; a small overlap check beneath the feet is often more robust.

## Overlap-box ground check
Attach to a player with Rigidbody2D and Collider2D. Create a child at the feet and assign it to groundCheck. Put solid ground on a Ground layer and include that layer in groundLayers.

    using UnityEngine;

    public sealed class GroundCheck2D : MonoBehaviour
    {
        [SerializeField] private Transform groundCheck;
        [SerializeField] private Vector2 checkSize = new Vector2(0.5f, 0.12f);
        [SerializeField] private LayerMask groundLayers;
        public bool IsGrounded { get; private set; }

        private void FixedUpdate()
        {
            if (groundCheck == null)
            {
                IsGrounded = false;
                return;
            }
            IsGrounded = Physics2D.OverlapBox(
                groundCheck.position, checkSize, 0f, groundLayers) != null;
        }

        private void OnDrawGizmosSelected()
        {
            if (groundCheck == null) return;
            Gizmos.color = Color.yellow;
            Gizmos.DrawWireCube(groundCheck.position, checkSize);
        }
    }

Tune the size so the check overlaps ground but does not extend high enough to treat a wall as ground. Exclude the player layer from the mask.

## LayerMask notes
A LayerMask is a bit mask, not simply a layer index. Use the Inspector to select layer names, or LayerMask.GetMask("Ground") in code. Verify that layer names exist and capitalization matches.

Common mistakes: Ground omitted from the mask, groundCheck embedded too high inside the player collider, check box too large and detecting walls, missing child Transform assignment, or collision settings preventing interaction.
