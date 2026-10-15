# 2D: Audio

## Guide
- [Audio setup and sound effects](./Audio-Setup-and-Sound-Effects.md)

## AudioSource basics
- AudioClip: imported audio asset.
- AudioSource: playback component.
- AudioListener: usually on the active camera; keep a single active listener.
- AudioMixer: groups sources and controls volume/effects centrally.

For positional 2D effects, Spatial Blend is normally 0. Disable Play On Awake for code-triggered effects. Use PlayOneShot for short overlapping sounds; use looping AudioSources for music/ambience as appropriate.

## Import and performance
Audio import settings trade memory, latency, and CPU use. Choose based on clip duration and target platform. Profile rather than selecting one setting for every sound.

## Troubleshooting
Check source/listener enabled states, assigned clips, source volume, mixer routing/mute, Spatial Blend, pause state, and platform audio settings.
