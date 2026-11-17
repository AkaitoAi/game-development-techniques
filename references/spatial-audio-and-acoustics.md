# Spatial audio and acoustics

## Problem and design reasoning

Straight-line distance can make an emitter behind a wall sound closer than one in an open doorway. Players may follow the wrong route even when left-right panning works. The Coalition and Microsoft Research used precomputed environmental propagation in Gears of War 4 and discussed interpreting the simulation for cinematic and emotional goals. The portable design inference is to tune propagation for intelligible routes and reserve controls for authored priorities. [Raghuvanshi, Microsoft Research](https://www.microsoft.com/en-us/research/podcast/project-triton-and-the-physics-of-sound-with-dr-nikunj-raghuvanshi/) · [The Coalition and Microsoft Research, GDC, Gears of War 4: Project Triton](https://www.gdcvault.com/play/1024650/-Gears-of-War-4)

## Shipped examples

- **Gears of War 4 (2016).** Project Triton precomputes wave propagation and reduces the results to parameters usable in a game. This makes effects of level geometry available during play without solving the full wave field at runtime. [Microsoft Research, Project Triton](https://www.microsoft.com/en-us/research/project/project-triton/)
- **Gears 5 (2019).** Microsoft describes combining scene-informed propagation with headphone rendering technologies. Geometry determines how sound arrives at the listener, and the output renderer translates that arrival into a playback format. [Microsoft Research development history](https://www.microsoft.com/en-us/research/blog/research-collection-the-unseen-history-of-audio-and-acoustics-research-at-microsoft/)

## Techniques and practical checks

### Keep listener orientation and propagation distinct

Compute the arrival direction in world space, then convert it into the chosen listener frame. Decide whether the view or controlled character supplies each part of the listener transform. Apply binaural filtering once where available and provide an appropriate stereo fallback.

**Check** a stationary source at several bearings and elevations while rotating the camera and character independently. Compare headphone and speaker modes for front-back errors, double processing, and abrupt pan changes.

### Route blocked sound through plausible openings

For a modest system, connect acoustic regions through openings. When the direct path is blocked, rank routes by length and attenuation, including a door's current open fraction. Smooth gain and filter settings when the chosen path changes. Preserve a low-level transmission path through materials if the design calls for it.

**Check** two parallel corridors, a shut door that opens slowly, and a source moving around a corner. Confirm the apparent route and filtering change smoothly, and verify that the sound remains findable when two paths have similar costs.

### Treat room response as a separate signal

Mix early reflections and late reverberation independently of the direct sound. Feed room response from acoustic regions or baked probes, and interpolate across doorways. Define the intended wet signal for backgrounds, voices, and gameplay cues.

**Check** one impulse and one sustained voice in an open area, a small room, and a connecting doorway. Listen for discontinuous tails and verify speech intelligibility across supported output modes.

### Budget updates around perceptual importance

Update nearby or gameplay-critical sources first, and amortize expensive queries for quiet background sources. Document which parts of the acoustic scene are baked and which respond to runtime geometry or door state.

**Check** a maximum-density scene with many emitters and several moving doors. Record query time and time until critical occlusion changes become audible. Compare against a reduced-quality tier and test whether essential signals remain spatially distinguishable.
