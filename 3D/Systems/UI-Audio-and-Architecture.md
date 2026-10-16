# 3D UI, Audio, and Reusable Architecture

This guide uses uGUI and TextMeshPro. The same separation of responsibilities also applies to UI Toolkit.

## AudioService3D.cs

~~~csharp
using UnityEngine;

public class AudioService3D : MonoBehaviour
{
    public static AudioService3D Instance { get; private set; }
    [SerializeField] private AudioSource effects, music;

    private void Awake()
    {
        if (Instance && Instance != this) { Destroy(gameObject); return; }
        Instance = this;
        DontDestroyOnLoad(gameObject);
    }

    public void PlayEffect(AudioClip clip, float volume = 1)
    {
        if (clip && effects) effects.PlayOneShot(clip, Mathf.Clamp01(volume));
    }
    public void PlayMusic(AudioClip clip)
    {
        if (!music || !clip) return;
        if (music.clip == clip && music.isPlaying) return;
        music.clip = clip; music.loop = true; music.Play();
    }
    public void StopMusic() { if (music) music.Stop(); }
}
~~~

Create one persistent AudioManager with effects and music AudioSources. For spatial 3D sounds, use an AudioSource attached to the world object and set Spatial Blend toward 3D. A global effects source is intentionally non-positional.

## HealthText3D.cs

~~~csharp
using TMPro;
using UnityEngine;

public class HealthText3D : MonoBehaviour
{
    [SerializeField] private Health3D target;
    [SerializeField] private TMP_Text label;
    private void OnEnable()
    {
        if (target) target.Changed += Refresh;
        if (target) Refresh(target.Current, target.Max);
    }
    private void OnDisable()
    {
        if (target) target.Changed -= Refresh;
    }
    private void Refresh(float current, float max)
    {
        if (label) label.text = $"Health: {current:0}/{max:0}";
    }
}
~~~

## Reusable architecture
- Health does not know which weapon caused damage or which UI displays it.
- Weapons depend on IDamageable, not a concrete enemy class.
- Events broadcast state changes; subscribers remove handlers in OnDisable.
- ScriptableObjects store authored data. Runtime health, ammo, cooldowns, and inventory quantities stay in runtime objects.
- Avoid mixing direct Transform movement, Rigidbody movement, and NavMeshAgent movement on the same actor.
- Use Assembly Definitions to separate Core, Gameplay, UI, and Infrastructure as the project grows.

### Menu setup
Create Canvas, EventSystem, Buttons, a pause panel, and TMP text. Attach SceneFlow3D and PauseController3D, assign references, and wire methods to Button OnClick. Route music/effects through AudioMixer groups when implementing settings sliders. Reset cursor lock and time scale when leaving pause.
