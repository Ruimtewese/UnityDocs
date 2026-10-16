# 2D Combat and Projectiles

Unity 6 uses Rigidbody2D, Collider2D, and Physics2D. This guide provides a shared damage contract, health, melee overlap checks, and projectiles.

## Damage contract — IDamageable.cs

~~~csharp
using UnityEngine;
public interface IDamageable
{
    void TakeDamage(float amount, Vector2 point, Vector2 impulse);
}
~~~

## Health — Health2D.cs

~~~csharp
using System;
using UnityEngine;

[RequireComponent(typeof(Collider2D))]
public class Health2D : MonoBehaviour, IDamageable
{
    [SerializeField, Min(1)] private float maxHealth = 5;
    [SerializeField] private Rigidbody2D body;
    public float Current { get; private set; }
    public float Max => maxHealth;
    public event Action<float, float> Changed;
    public event Action Died;
    private bool dead;

    private void Awake()
    {
        if (!body) body = GetComponent<Rigidbody2D>();
        Current = maxHealth;
        Changed?.Invoke(Current, maxHealth);
    }

    public void TakeDamage(float amount, Vector2 point, Vector2 impulse)
    {
        if (dead || amount <= 0) return;
        Current = Mathf.Max(0, Current - amount);
        if (body && body.bodyType == RigidbodyType2D.Dynamic)
            body.AddForce(impulse, ForceMode2D.Impulse);
        Changed?.Invoke(Current, maxHealth);
        if (Current <= 0)
        {
            dead = true;
            Died?.Invoke();
            Destroy(gameObject);
        }
    }

    public void Heal(float amount)
    {
        if (dead || amount <= 0) return;
        Current = Mathf.Min(maxHealth, Current + amount);
        Changed?.Invoke(Current, maxHealth);
    }
}
~~~

## Melee — MeleeAttack2D.cs

~~~csharp
using System.Collections.Generic;
using UnityEngine;

public class MeleeAttack2D : MonoBehaviour
{
    [SerializeField] private Transform origin;
    [SerializeField] private float radius = 0.7f, damage = 1, knockback = 3;
    [SerializeField] private LayerMask targets;

    public void Attack()
    {
        if (!origin) return;
        var alreadyHit = new HashSet<IDamageable>();
        foreach (Collider2D hit in Physics2D.OverlapCircleAll(origin.position, radius, targets))
        {
            if (hit.transform.root == transform.root) continue;
            IDamageable target = hit.GetComponentInParent<IDamageable>();
            if (target == null || !alreadyHit.Add(target)) continue;
            Vector2 dir = ((Vector2)hit.bounds.center - (Vector2)origin.position).normalized;
            target.TakeDamage(damage, hit.ClosestPoint(origin.position), dir * knockback);
        }
    }

    private void OnDrawGizmosSelected()
    {
        if (origin) Gizmos.DrawWireSphere(origin.position, radius);
    }
}
~~~

Call Attack once from an input action or animation event, not every frame while a key is held.

## Projectile — Projectile2D.cs

~~~csharp
using UnityEngine;

[RequireComponent(typeof(Rigidbody2D), typeof(Collider2D))]
public class Projectile2D : MonoBehaviour
{
    [SerializeField] private float lifetime = 4;
    private Rigidbody2D body;
    private Transform owner;
    private Vector2 direction;
    private float damage, knockback;
    private bool spent;

    private void Awake()
    {
        body = GetComponent<Rigidbody2D>();
        GetComponent<Collider2D>().isTrigger = true;
    }

    public void Launch(Transform source, Vector2 aim, float speed, float damageValue, float force)
    {
        owner = source; direction = aim.normalized;
        damage = damageValue; knockback = force;
        body.gravityScale = 0;
        body.linearVelocity = direction * speed;
        Destroy(gameObject, lifetime);
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (spent || (owner && other.transform.root == owner.root)) return;
        IDamageable target = other.GetComponentInParent<IDamageable>();
        if (target == null) return;
        spent = true;
        target.TakeDamage(damage, other.ClosestPoint(transform.position), direction * knockback);
        Destroy(gameObject);
    }
}
~~~

## Launcher — ProjectileLauncher2D.cs

~~~csharp
using UnityEngine;

public class ProjectileLauncher2D : MonoBehaviour
{
    [SerializeField] private Projectile2D prefab;
    [SerializeField] private Transform muzzle;
    [SerializeField] private float speed = 12, damage = 1, knockback = 2;

    public void Fire(Vector2 direction)
    {
        if (!prefab || !muzzle || direction.sqrMagnitude < 0.001f) return;
        Projectile2D shot = Instantiate(prefab, muzzle.position, Quaternion.identity);
        shot.Launch(transform, direction, speed, damage, knockback);
    }
}
~~~

### Setup and debugging
- Projectile prefab: SpriteRenderer, Rigidbody2D (Gravity Scale 0; Collision Detection Continuous), CircleCollider2D (Is Trigger), Projectile2D.
- Assign prefab and muzzle to launcher. The target needs a Collider2D and a component implementing IDamageable.
- Put targets on a layer included in the melee LayerMask. Configure Physics 2D layer interactions.
- Trigger callbacks require a Rigidbody2D in the pair.
- This projectile stops on damageable objects; add explicit wall handling if needed.
- Unity 6 uses Rigidbody2D.linearVelocity; older versions may expose velocity instead.
