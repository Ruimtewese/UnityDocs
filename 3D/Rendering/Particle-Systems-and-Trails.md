# 3D Particle Systems and Trails

Particle Systems create effects such as dust, sparks, smoke, and magic-like visuals. Trail Renderers draw a fading strip behind a moving object. Prefer lightweight, pooled effects when many effects spawn frequently.

## Particle System setup
Create GameObject → Effects → Particle System. Inspect:
- Main: Duration, Looping, Start Lifetime, Start Speed, Start Size, Start Color, Simulation Space, and Scaling Mode.
- Emission: rate over time and bursts.
- Shape: initial spread/shape.
- Color over Lifetime / Size over Lifetime: fade or shrink over the particle lifetime.
- Renderer: render mode, material, sorting, and alignment.
- Collision: optional and potentially expensive; enable only if particles need it.

Labels/modules can vary depending on Unity version. Disable modules not used by the effect.

## Play from code
Attach this to the effect GameObject with a ParticleSystem.

    using UnityEngine;

    [RequireComponent(typeof(ParticleSystem))]
    public sealed class ParticleEffectPlayer : MonoBehaviour
    {
        [SerializeField] private ParticleSystem effect;

        private void Awake()
        {
            if (effect == null) effect = GetComponent<ParticleSystem>();
            if (effect != null) effect.Stop(true, ParticleSystemStopBehavior.StopEmittingAndClear);
        }

        public void PlayAt(Vector3 position)
        {
            if (effect == null) return;
            transform.position = position;
            effect.Stop(true, ParticleSystemStopBehavior.StopEmittingAndClear);
            effect.Play(true);
        }
    }

For pooled effects, reset position, rotation, parent, and particle state before replay. If the particle system is a child of a moving object, choose Simulation Space deliberately: Local follows the parent; World continues in world space.

## Trails
TrailRenderer has time, width, color/gradient, material, alignment, and emitting controls. Clear the trail when reusing a pooled object if old segments should not remain visible. Tune time and width to prevent excessive overdraw.

## Performance tips
Limit particle count, avoid expensive collision modules unnecessarily, use appropriate materials, profile transparent overdraw, and pool high-frequency effects.
