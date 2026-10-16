# 2D UI, Audio, and Reusable Architecture

Use uGUI or UI Toolkit for menus. This example uses uGUI buttons and TextMeshPro for health text.

## AudioService2D.cs

~~~csharp
using UnityEngine;

public class AudioService2D : MonoBehaviour
{
    public static AudioService2D Instance { get; private set; }
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

Create one AudioManager with two AudioSources, assign them, and avoid placing a new manager in every scene. Duplicate instances self-destruct.

## Health UI — HealthText2D.cs

~~~csharp
using TMPro;
using UnityEngine;

public class HealthText2D : MonoBehaviour
{
    [SerializeField] private Health2D target;
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

## Architecture rules
- Health owns health; weapons request damage; UI displays state.
- Use IDamageable rather than making weapons depend on one enemy class.
- Use events and unsubscribe in OnDisable.
- ScriptableObjects hold authored data (item definitions, weapon tuning, enemy configuration); runtime ammo and health live on components/models.
- Prefer explicit Inspector references over repeated scene-wide searches.
- Keep global services limited. Scene-owned components should remain scene-owned.
- For larger projects, use Assembly Definition files to separate Core, Gameplay, UI, and Infrastructure.

### Menu wiring
Create a Canvas, Button, pause panel, TMP text, and EventSystem. Attach SceneFlow2D and PauseController2D to scene objects, then assign their public methods to Button OnClick events. Add an AudioSource for effects and another for music. If using volume sliders, route sources through AudioMixer groups.
