# 3D Spatial Audio

## Setup
Keep one active AudioListener, typically on the player camera. Add an AudioSource to the sound-producing object, assign a clip, and disable Play On Awake for code-triggered playback. Set Spatial Blend toward 1 for 3D positioning. Configure Min Distance, Max Distance, and rolloff for the scale of the world.

## Sound emitter
Save as SpatialSoundEmitter.cs and attach beside an AudioSource.

    using UnityEngine;
    [RequireComponent(typeof(AudioSource))]
    public sealed class SpatialSoundEmitter : MonoBehaviour
    {
        [SerializeField] private AudioClip clip;
        [SerializeField, Range(0f, 1f)] private float volume = 1f;
        private AudioSource source;
        private void Awake()
        {
            source = GetComponent<AudioSource>();
            source.spatialBlend = 1f;
            source.playOnAwake = false;
        }
        public void Play()
        {
            if (clip != null) source.PlayOneShot(clip, volume);
        }
    }

Use an AudioMixer to group SFX, dialogue, and music. Expose mixer parameters for settings; changing one AudioSource volume does not change every source.

Common mistakes: missing or multiple listeners, Spatial Blend set to 0, incorrect distance/rolloff, source on the wrong transform, or a muted mixer/listener.
