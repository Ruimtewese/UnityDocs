# Build and Performance Checklist

## Before building
- Fix Console errors and review warnings.
- Include required scenes in the active Build Profile.
- Select the target platform and architecture.
- Check input backend and package compatibility.
- Verify textures, audio, materials, fonts, and shader variants.
- Test pause/resume, window focus, aspect ratios, and controller disconnects where relevant.
- Remove development-only shortcuts and secrets.

## Performance workflow
1. Establish a baseline on target hardware.
2. Profile CPU, GPU, memory, garbage collection, physics, and rendering.
3. Identify the dominant cost.
4. Change one thing at a time.
5. Profile again and check regressions.

Common bottlenecks include per-frame allocations, excessive Instantiate/Destroy, too many physics bodies, real-time lights/shadows, transparent overdraw, large assets loaded unnecessarily, frequent UI rebuilds, scene-wide searches, and overly frequent AI updates.

## Build failures
Read the earliest error. Check package versions, scripting defines, platform support, API compatibility, and Editor-only namespaces. Reimporting everything should not be the first response.

## Release checklist
- [ ] Clean Console and successful target build
- [ ] Menu, restart, pause/resume, and transitions tested
- [ ] Resolutions and aspect ratios tested
- [ ] Save/load tested with missing or invalid data
- [ ] Audio settings and focus behavior tested
- [ ] No runtime dependency on Editor-only objects
- [ ] Performance checked on target hardware
- [ ] Build version and known limitations documented
