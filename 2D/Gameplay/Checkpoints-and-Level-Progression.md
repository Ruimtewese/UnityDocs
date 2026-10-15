# 2D Checkpoints and Level Progression

A checkpoint stores a respawn position after the player touches it. Keep checkpoint state separate from its visual indicator. Decide whether progress resets after scene reload or persists in a save file.

## Checkpoint trigger
Attach this script to an object with Collider2D set to Is Trigger. Assign a player tag and optionally a child respawnPoint Transform.

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

## Player respawn component
Attach to the player root. Assign initialSpawnPoint if the initial spawn differs from the player's starting Transform.

    using UnityEngine;

    public sealed class PlayerRespawn2D : MonoBehaviour
    {
        [SerializeField] private Transform initialSpawnPoint;
        [SerializeField] private Rigidbody2D body;
        private Vector2 checkpointPosition;

        private void Awake()
        {
            if (body == null) body = GetComponent<Rigidbody2D>();
            checkpointPosition = initialSpawnPoint != null
                ? (Vector2)initialSpawnPoint.position
                : (Vector2)transform.position;
        }

        public void SetCheckpoint(Vector2 position) => checkpointPosition = position;

        public void Respawn()
        {
            if (body != null)
            {
                body.linearVelocity = Vector2.zero;
                body.angularVelocity = 0f;
                body.position = checkpointPosition;
            }
            else
            {
                transform.position = checkpointPosition;
            }
        }
    }

Call Respawn when health reaches zero. Decide separately whether to restore health, lives, and collected items. Using a nullable Transform instead of a zero-coordinate sentinel allows the world origin to be a valid spawn.

## Scene progression
Use SceneManager and include scenes in the active Build Profile. Store the next scene name/index deliberately; Project folder order does not determine build index.

## Common mistakes
- Trigger never fires because collider/layer/Rigidbody setup is wrong.
- Player tag does not match.
- Respawn leaves Rigidbody velocity intact.
- Health/death events are not connected to Respawn.
- Scene name or build index is not configured.
