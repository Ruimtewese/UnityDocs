# 2D Audio: Music and Sound Effects

AudioListener is usually on the camera; keep one active listener. AudioSource plays clips and configures volume, pitch, looping, spatial blend, and mixer routing. AudioMixer groups sources and controls effects/volume centrally. For conventional 2D effects, set Spatial Blend to 0.

## One-shot service
Create an AudioService object, add an AudioSource, disable Play On Awake, and assign a short confirm clip. Save this as AudioService.cs.

    using UnityEngine;
    public sealed class AudioService : MonoBehaviour
    {
        public static AudioService Instance { get; private set; }
        [SerializeField] private AudioSource source;
        [SerializeField] private AudioClip confirmClip;
        [SerializeField, Range(0f, 1f)] private float volume = 1f;
        private void Awake()
        {
            if (Instance != null && Instance != this)
            {
                Destroy(gameObject);
                return;
            }
            Instance = this;
            if (source == null) source = GetComponent<AudioSource>();
        }
        public void PlayConfirm()
        {
            if (source != null && confirmClip != null)
                source.PlayOneShot(confirmClip, volume);
        }
    }

Call AudioService.Instance?.PlayConfirm() from UI/gameplay code. This sample does not persist across scenes. If adding DontDestroyOnLoad, prevent duplicates and deliberately manage lifetime.

## Import and performance
Short effects may use Decompress On Load or Compressed In Memory depending on latency, memory, and CPU. Long music often suits streaming or compressed-in-memory settings. Profile on target hardware. Pool or cap sources for rapid repeated sounds.

Common mistakes: no listener, multiple listeners, no clip/play call, Spatial Blend incorrectly set, mixer muted, or playback occurring while audio is paused.
