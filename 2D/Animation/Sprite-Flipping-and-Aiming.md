# 2D Sprite Flipping and Aiming

## Flip a character horizontally
Use SpriteRenderer.flipX when only the rendered sprite needs to face the other direction. This avoids changing Transform scale and introducing negative-scale side effects.

    using UnityEngine;

    public sealed class SpriteFacing2D : MonoBehaviour
    {
        [SerializeField] private SpriteRenderer spriteRenderer;
        [SerializeField, Min(0f)] private float deadZone = 0.05f;

        private void Awake()
        {
            if (spriteRenderer == null) spriteRenderer = GetComponentInChildren<SpriteRenderer>();
        }

        public void SetHorizontalMove(float x)
        {
            if (spriteRenderer == null || Mathf.Abs(x) < deadZone) return;
            spriteRenderer.flipX = x < 0f;
        }
    }

Call SetHorizontalMove with the character's horizontal movement input or velocity. If the character has a weapon/child transform that must also face the aim direction, flip or rotate that transform separately.

## Aim toward mouse
For a 2D world on the XY plane and a standard camera facing that plane, convert the mouse to world coordinates. Use camera-space depth and check the returned coordinate plane. A simple screen-to-world conversion must use a depth that intersects the gameplay plane.

    Vector3 mouse = Camera.main.ScreenToWorldPoint(
        new Vector3(Mouse.current.position.ReadValue().x,
                    Mouse.current.position.ReadValue().y,
                    Mathf.Abs(Camera.main.transform.position.z)));
    Vector2 aim = (Vector2)mouse - (Vector2)weapon.position;
    if (aim.sqrMagnitude > 0.0001f)
        weapon.right = aim.normalized;

This snippet assumes a conventional 2D camera at negative Z and gameplay around Z = 0. It requires the Input System namespace and a Mouse device. For gamepad aiming, use a direction vector from the stick instead.

## Common mistakes
- Sprite is mirrored but colliders/child weapon are not positioned as intended.
- Transform scale is negated and creates hierarchy side effects.
- Mouse depth does not intersect the gameplay plane.
- Code rotates the whole Rigidbody root and changes physics behavior.
