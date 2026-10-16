# 3D Combat and Projectiles

This Unity 6 example uses Collider, Rigidbody, Physics, and a shared IDamageable contract.

## IDamageable.cs

~~~csharp
using UnityEngine;
public interface IDamageable
{
    void TakeDamage(float amount, Vector3 point, Vector3 impulse);
}
~~~

## Health3D.cs

~~~csharp
using System;
using UnityEngine;

[RequireComponent(typeof(Collider))]
public class Health3D : MonoBehaviour, IDamageable
{
    [SerializeField, Min(1)] private float maxHealth = 10;
    [SerializeField] private Rigidbody body;
    public float Current { get; private set; }
    public float Max => maxHealth;
    public event Action<float, float> Changed;
    public event Action Died;
    private bool dead;

    private void Awake()
    {
        if (!body) body = GetComponent<Rigidbody>();
        Current = maxHealth;
        Changed?.Invoke(Current, maxHealth);
    }

    public void TakeDamage(float amount, Vector3 point, Vector3 impulse)
    {
        if (dead || amount <= 0) return;
        Current = Mathf.Max(0, Current - amount);
        if (body && !body.isKinematic)
            body.AddForceAtPosition(impulse, point, ForceMode.Impulse);
        Changed?.Invoke(Current, maxHealth);
        if (Current <= 0)
        {
            dead = true; Died?.Invoke(); Destroy(gameObject);
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

## MeleeAttack3D.cs

~~~csharp
using System.Collections.Generic;
using UnityEngine;

public class MeleeAttack3D : MonoBehaviour
{
    [SerializeField] private Transform origin;
    [SerializeField] private float radius = 1, damage = 2, knockback = 4;
    [SerializeField] private LayerMask targets;

    public void Attack()
    {
        if (!origin) return;
        var alreadyHit = new HashSet<IDamageable>();
        foreach (Collider hit in Physics.OverlapSphere(origin.position, radius, targets))
        {
            if (hit.transform.root == transform.root) continue;
            IDamageable target = hit.GetComponentInParent<IDamageable>();
            if (target == null || !alreadyHit.Add(target)) continue;
            Vector3 dir = (hit.bounds.center - origin.position).normalized;
            target.TakeDamage(damage, hit.ClosestPoint(origin.position), dir * knockback);
        }
    }

    private void OnDrawGizmosSelected()
    {
        if (origin) Gizmos.DrawWireSphere(origin.position, radius);
    }
}
~~~

## Projectile3D.cs

~~~csharp
using UnityEngine;

[RequireComponent(typeof(Rigidbody), typeof(Collider))]
public class Projectile3D : MonoBehaviour
{
    [SerializeField] private float lifetime = 5;
    private Rigidbody body;
    private Transform owner;
    private Vector3 direction;
    private float damage, knockback;
    private bool spent;

    private void Awake()
    {
        body = GetComponent<Rigidbody>();
        body.useGravity = false;
        GetComponent<Collider>().isTrigger = true;
    }

    public void Launch(Transform source, Vector3 aim, float speed, float damageValue, float force)
    {
        owner = source; direction = aim.normalized;
        damage = damageValue; knockback = force;
        body.linearVelocity = direction * speed;
        Destroy(gameObject, lifetime);
    }

    private void OnTriggerEnter(Collider other)
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

## ProjectileLauncher3D.cs

~~~csharp
using UnityEngine;

public class ProjectileLauncher3D : MonoBehaviour
{
    [SerializeField] private Projectile3D prefab;
    [SerializeField] private Transform muzzle;
    [SerializeField] private float speed = 20, damage = 2, knockback = 3;

    public void Fire(Vector3 direction)
    {
        if (!prefab || !muzzle || direction.sqrMagnitude < 0.001f) return;
        Projectile3D shot = Instantiate(prefab, muzzle.position, Quaternion.LookRotation(direction));
        shot.Launch(transform, direction, speed, damage, knockback);
    }
}
~~~

### Setup
Projectile prefab: visible mesh, Rigidbody (Use Gravity off; Collision Detection Continuous), SphereCollider (Is Trigger), Projectile3D. Assign prefab/muzzle to launcher. Targets need colliders and IDamageable. Use a target LayerMask for melee and configure collision layers.

The projectile ignores its owner and is destroyed on the first damageable hit. Add wall collision handling for your game. Unity 6 uses Rigidbody.linearVelocity; older versions may use velocity.
