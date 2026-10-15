# 3D UI: Settings Menu and Audio Volume

A settings menu often controls master volume, graphics options, sensitivity, and control preferences. This guide demonstrates a Slider with an AudioMixer exposed parameter. It uses uGUI and TextMeshPro only for the surrounding menu; the volume hookup is independent of the project layout.

## AudioMixer setup
1. Create an Audio Mixer asset and a group for your game audio.
2. Route AudioSources to the group.
3. In the mixer, expose a parameter such as MasterVolume.
4. Use the exact exposed parameter name in the script below.
5. Add a Slider to the settings menu and connect its On Value Changed event, or assign the Slider in the Inspector.

## Volume controller
Save as AudioVolumeSetting.cs.

    using UnityEngine;
    using UnityEngine.Audio;
    using UnityEngine.UI;

    public sealed class AudioVolumeSetting : MonoBehaviour
    {
        [SerializeField] private AudioMixer mixer;
        [SerializeField] private Slider slider;
        [SerializeField] private string exposedParameter = "MasterVolume";

        private void Start()
        {
            if (slider != null)
            {
                slider.minValue = 0.0001f;
                slider.maxValue = 1f;
                slider.onValueChanged.AddListener(SetLinearVolume);
                SetLinearVolume(slider.value);
            }
        }

        public void SetLinearVolume(float linear)
        {
            if (mixer == null) return;
            float decibels = 20f * Mathf.Log10(Mathf.Clamp(linear, 0.0001f, 1f));
            mixer.SetFloat(exposedParameter, decibels);
        }

        private void OnDestroy()
        {
            if (slider != null) slider.onValueChanged.RemoveListener(SetLinearVolume);
        }
    }

AudioMixer volume parameters normally use decibels; the script converts a linear Slider value. The exact exposed parameter must exist or SetFloat will not control the intended setting. A small minimum avoids log10(0); if the UI needs true mute, add a separate mute toggle.

## Persistence
To remember settings, save the linear slider value in the player's preferences or a save model, then restore it before first playback. PlayerPrefs is appropriate for simple preferences, not secrets or large save data.

## Common mistakes
- Mixer parameter wasn't exposed or the name is incorrect.
- AudioSources aren't routed to the controlled mixer group.
- Linear slider value is written directly as a decibel value.
- Listener or group is muted elsewhere.
