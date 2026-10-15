# 3D: Audio

## Positional sound
AudioSource Spatial Blend controls 2D/3D behaviour. 3D settings can include volume rolloff, minimum/maximum distance, spread, and Doppler level. Position emitters where the sound should originate and keep them attached to moving sources when appropriate.

## AudioMixer routing
Separate Music, SFX, ambience, and UI into mixer groups when you need independent volume controls or effects. Keep a deliberate AudioListener on the active camera or another suitable object.

## Troubleshooting
- Check AudioListener presence and avoid multiple active listeners.
- Check source volume, clip, mixer group, mute state, and rolloff distances.
- Confirm the AudioSource is at the intended world position.
- Check Play On Awake when spawned objects play duplicate sounds.

Reuse or pool frequent emitters when profiling supports it.

Reference: [AudioSource API](https://docs.unity3d.com/ScriptReference/AudioSource.html)
