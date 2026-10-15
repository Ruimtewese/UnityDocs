# 3D: Audio

## Guide
- [3D spatial audio](./3D-Spatial-Audio.md)

## Spatial setup
Keep one active AudioListener, normally on the player camera. Set a world-space AudioSource's Spatial Blend toward 1, configure Min/Max Distance and rolloff, and place the source on the object producing the sound.

## Mixing
Route sources to AudioMixer groups for SFX, music, dialogue, and ambience. Expose mixer parameters for settings controls. Convert linear UI volume to decibels when controlling an exposed mixer volume parameter.

## Troubleshooting
If a sound is silent, check clip/source enabled state, Play On Awake/manual Play call, listener, source and mixer volume, spatial distance/rolloff, AudioListener pause, and whether the object has been disabled/destroyed.
