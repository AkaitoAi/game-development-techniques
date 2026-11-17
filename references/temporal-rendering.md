# Temporal rendering and image reconstruction

## Problem and design reasoning

**Problem.** Thin details shimmer in motion, a trail follows moving objects, or quality drops when dynamic resolution changes. Inspect motion data and history validity at cuts and newly visible surfaces.

**Design reasoning.** Epic documents combining temporal upsampling and dynamic resolution to target 60 Hz console play. The inferred goal is to reuse stable prior samples to meet the budget and discard them where correspondence has broken. See the shipped examples below for sources.

## Shipped examples

**Uncharted 4: A Thief's End (2016).** Naughty Dog used temporal anti-aliasing to address aliasing that persists when material highlights or shading features move between pixels. Its SIGGRAPH talk describes the implementation and the problems encountered across varied game environments. [Ke Xu, SIGGRAPH 2016](https://advances.realtimerendering.com/s2016/index.html)

**Fortnite Battle Royale (2017 onward).** Epic's SIGGRAPH 2018 course identifies the combination of temporal upsampling and dynamic resolution shipped for 60 Hz console play. Chapter 4 later shipped Temporal Super Resolution on supported configurations. The case illustrates using history to reconstruct a high-resolution output from a lower-resolution internal render while adjusting input resolution to fit a GPU budget. [Guillaume Abadie, SIGGRAPH 2018](https://advances.realtimerendering.com/s2018/index.htm) · [Epic, Chapter 4 release notes](https://www.fortnite.com/news/drop-into-the-next-generation-of-fortnite-battle-royale-powered-by-unreal-engine-5-1?lang=en-US)

## Techniques and practical checks

### Jitter the render and reproject valid history

Use a bounded sequence of subpixel offsets in the projection. Define a single convention for motion vector direction, units, jitter, and render versus output coordinates. Derive each sample's previous position from last-frame camera and actor state. Include deformation for assets whose silhouettes move independently of their object transforms. Blend history in a consistent exposure space.

**Check** a static diagonal edge, a slow camera pan, a moving rigid actor, and a wind-deformed mesh. Inspect the velocity buffer and the reprojected sample coordinates. A stationary view should converge while moving edges track their new positions.

### Reject or constrain stale samples

Lower history weight where previous depth and current geometry disagree, where reprojection goes outside the frame, or where shading changes faster than motion vectors can represent. Constrain history with a local current-frame neighborhood where appropriate. Clear history on camera cuts and scene switches. Choose independent tolerances for edges, thin features, and highly reflective materials.

**Check** a foreground object revealing a patterned wall, a teleport, a reflective surface under a moving light, and a sudden exposure shift. Capture consecutive frames and inspect ghost trails, flicker, and time to reconverge.

### Treat transparency and overlays explicitly

Choose a history policy for particles, alpha-tested surfaces, and emissive effects. If available, provide their motion or a reactive mask. Composite screen-space text and interface elements after temporal reconstruction where possible so output resolution changes preserve legibility.

**Check** readable text while resolution changes, fast particles against a contrasting wall, and foliage viewed in motion. Compare separate frame captures for retained detail, blur, and duplicate edges.

### Couple reconstruction to a measured resolution controller

Measure the GPU work affected by internal resolution. Adjust scale within tested limits using smoothing, hysteresis, and a minimum scale appropriate for the content. Define whether supported scale changes preserve or reset history. Compare output against native rendering under the same timing conditions.

**Check** alternating low- and high-cost views and force several resolution steps. Record internal scale, reconstruction cost, high-percentile frame time, and visible instability. Verify performance and readability on each supported target tier.
