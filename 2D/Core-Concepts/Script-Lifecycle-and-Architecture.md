# Unity C# Lifecycle and Script Architecture

| Callback | Typical purpose | Important detail |
|---|---|---|
| Awake | Cache components and initialize local state | Do not assume other objects have run Start |
| OnEnable | Subscribe to events or enable actions | Runs on each enable |
| Start | Setup needing other objects' Awake initialization | Runs once before first frame update while enabled |
| Update | Frame-based input and non-physics logic | Frame rate varies |
| FixedUpdate | Physics-step work | Zero, one, or multiple times per rendered frame |
| LateUpdate | Camera follow after ordinary updates | Other-script ordering may still need design |
| OnDisable | Unsubscribe and disable actions | Can run many times |
| OnDestroy | Final cleanup | Other objects may already be gone |

Collision and trigger callbacks require correct colliders, Rigidbody configuration, and matching 2D/3D physics types.

## Good habits
- Cache components in Awake instead of searching every frame.
- Keep movement, health, UI, and audio responsibilities separate.
- Use private fields with SerializeField for Inspector configuration.
- Validate references and log clear errors.
- Subscribe in OnEnable and unsubscribe in OnDisable if re-enable cycles are possible.
- Capture short-lived input in Update or action callbacks, then pass it to physics logic.
- Do not rely on unspecified order between unrelated scripts.

## Component pattern

    using UnityEngine;
    public sealed class ExampleComponent : MonoBehaviour
    {
        [SerializeField] private Rigidbody2D body;
        [SerializeField, Min(0f)] private float moveSpeed = 5f;
        private void Awake()
        {
            if (body == null) body = GetComponent<Rigidbody2D>();
            if (body == null) Debug.LogError("Rigidbody2D is required.", this);
        }
        private void Update()
        {
            // Read frame-based input here.
        }
        private void FixedUpdate()
        {
            if (body == null) return;
            // Apply physics changes here.
        }
    }

Update runs per rendered frame; FixedUpdate uses the fixed timestep in Time settings. Never assume they run one-to-one. Coroutines run on Unity's main loop and are not threads. WaitForSeconds is scaled by timeScale; WaitForSecondsRealtime is not.
