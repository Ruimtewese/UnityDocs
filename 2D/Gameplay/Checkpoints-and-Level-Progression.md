# 2D Checkpoints and Level Progression

## Design
A checkpoint stores a respawn position after the player touches it. Keep checkpoint state separate from the visual checkpoint indicator. Decide whether progress resets on scene reload or persists in a save file.

## Checkpoint
Attach to a checkpoint object with a Collider2D set to Is Trigger. The player needs the expected tag.

    using UnityEngine;

    public sealed class Checkpoint2D : MonoBehaviour
    {
        [SerializeField] private string playerTag = "Player";
        [SerializeField] private Transform respawnPoint;
        [SerializeField] private GameObject activatedVisual;
        private bool activated;

        private void OnTriggerEnter2D(Collider2D other)
        {
            if (activated || !other.CompareTag(playerTag)) return;

            PlayerRespawn2D respawn = other.GetComponentInParent<PlayerRespawn2D>();
            if (respawn == null) return;

            Transform point = respawnPoint != null ? respawnPoint : transform;
            respawn.SetCheckpoint(point.position);
            activated = true;
            if (activatedVisual != null) activatedVisual.SetActive(true);
        }
    }

Attach this component to the player or its root.

    using UnityEngine;

    public sealed class PlayerRespawn2D : MonoBehaviour
    {
        [SerializeField] private Vector2 startingPosition;
        [SerializeField] private Rigidbody2D body;
        private Vector2 checkpointPosition;

        private void Awake()
        {
            if (body == null) body = GetComponent<Rigidbody2D>();
            checkpointPosition = startingPosition == Vector2.zero
                ? (Vector2)transform.position
                : startingPosition;
        }

        public void SetCheckpoint(Vector2 position) => checkpointPosition = position;

        public void Respawn()
        {
            if (body != null)
            {
                body.linearVelocity = Vector2.zero;
                body.position = checkpointPosition;
            }
            else transform.position = checkpointPosition;
        }
    }

The zero-vector convention above makes world origin ambiguous. For a production setup, use an explicit bool such as hasStartingPosition or initialize the checkpoint from transform.position only. Call Respawn when health reaches zero; decide separately whether to restore health, lives, or collected items.

## Scene progression
Use SceneManager for level transitions, and ensure scenes are included in the active Build Profile. Store the next scene name/index deliberately; do not assume Project folder order determines build order.

## Common mistakes
- Trigger never fires because collider/layer/Rigidbody setup is incorrect.
- Player tag does not match.
- Respawn leaves Rigidbody velocity intact, causing immediate movement.
- Health/death events are not connected to Respawn.
- Scene name or build index is not configured.
