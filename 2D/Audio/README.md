# 2D: Audio

## AudioSource settings
Review Audio Clip, Output, Volume, Pitch, Loop, Play On Awake, Spatial Blend, and Priority. For traditional non-positional 2D sound, Spatial Blend is normally set toward 2D.

## AudioMixer
Route Music, SFX, and UI audio through separate mixer groups when you need independent volume sliders or effects. Expose mixer parameters for controlled runtime adjustment.

## Good practices
- Use Play On Awake only when automatic playback is intended.
- Reuse or pool frequent one-shot emitters if profiling supports it.
- Keep music, UI feedback, and gameplay sound effects independently adjustable.
- Check AudioListener presence, mixer mute/volume, and clip import settings when sound is missing.

Reference: [AudioSource API](https://docs.unity3d.com/ScriptReference/AudioSource.html)
