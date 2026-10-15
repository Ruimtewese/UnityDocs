# 2D Physics Queries and Collision Filtering

## Collision Matrix
Open **Project Settings → Physics 2D** and inspect the Layer Collision Matrix. A pair of layers with its box unchecked will not produce normal physics interactions. A GameObject's Tag does not replace its Layer.

## LayerMask
Use a LayerMask to select which colliders a raycast, cast, or overlap query may detect. This is more reliable than finding any hit and then guessing what it represents.

## Common query choices
- `Physics2D.Raycast`: thin line query, useful for aiming or checking distance to a surface.
- `Physics2D.CircleCast`: sweeps a circle; useful for a rounded ground probe.
- `Physics2D.BoxCast`: sweeps a box-shaped volume.
- `Physics2D.OverlapCircle`: checks an area at a single position.
- `Physics2D.OverlapBox`: checks a rectangular area.

## Example: ground overlap

```csharp
[SerializeField] private Transform feet;
[SerializeField] private float radius = 0.12f;
[SerializeField] private LayerMask groundLayers;

private bool IsGrounded()
{
    return feet != null &&
        Physics2D.OverlapCircle(feet.position, radius, groundLayers);
}

private void OnDrawGizmosSelected()
{
    if (feet == null) return;
    Gizmos.DrawWireSphere(feet.position, radius);
}
```

## Trigger vs collision
Use a trigger for pickups, proximity zones, checkpoints, and detection volumes where the overlap should not physically block movement. Use a solid collider for walls and floors. Exact callback requirements depend on collider and Rigidbody2D configuration; check the official documentation for the setup used.

## Common failures
- LayerMask does not include the intended ground layer.
- The probe starts above or inside an unexpected collider.
- Collider is disabled or on an inactive GameObject.
- The object is on the wrong Layer despite having the correct Tag.
- Composite Collider 2D is configured without the required Rigidbody2D/participating collider settings.

Reference: [Physics2D API](https://docs.unity3d.com/ScriptReference/Physics2D.html)
