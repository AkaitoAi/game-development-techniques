# Technical art

## Problem and design reasoning

**Problem.** A rendering technique works in a showcase asset but artists cannot reproduce it across a level, or content breaks runtime budgets late in production. Compare default assets with extremes of scale, shape, and density.

**Design reasoning.** Guerrilla documents converting offline wave data into a reusable, artist-controllable runtime system. The inferred trade-off is to expose controls artists can judge while baking or bounding the expensive simulation behind those controls. See the shipped examples below for sources.

## Shipped examples

**Team Fortress 2 (2007).** Valve's NPAR paper presents illustrative rendering as a collaboration between asset design and lighting. A cool-to-warm shading model and rim highlights support the distinction between character silhouettes and surrounding scenery. A look can be established through an explicit rendering and asset convention that artists can apply consistently. [Valve developers, Illustrative Rendering in Team Fortress 2, NPAR 2007](https://cdn.steamstatic.com/apps/valve/2007/NPAR07_IllustrativeRenderingInTeamFortress2.pdf)

**Horizon Forbidden West (2022).** Guerrilla's SIGGRAPH presentation explains how offline water simulations were processed into localized surface deformations, assembled into wavefronts, and exposed to artists through controls for shape and animation. The combination of reusable runtime data and art direction controls made a costly source process available as a practical production technique. [Guerrilla, Horizon Forbidden West water rendering, SIGGRAPH 2022](https://www.realtimerendering.com/advances/s2022/index.html)

## Techniques and practical checks

### Establish a look with reference assets

Pick one representative asset and a small reference scene with controlled exposure, light direction, and game camera distances. Define which visual cues must survive production changes, such as readable character silhouettes or clear edge contrast. Document which cues come from meshes, textures, materials, and lighting so artists can adjust the right inputs.

**Check** by placing multiple characters against dark, bright, and busy backgrounds and reviewing frames at actual gameplay size. Compare near and far silhouettes across the supported quality tiers.

### Convert expensive source effects to reusable runtime data

Identify what the runtime has to reproduce and what artists need to vary. Bake reusable deformation clips, maps, or curves when offline simulation would exceed the frame budget. Preserve a documented mapping from source units, time, and coordinate space to the playback system. Expose controls for variation and reusable placement.

**Check** by placing several instances with different timings and scales. Compare a capture with the source reference, inspect seams and looping, and measure runtime memory and GPU cost at the busiest supported density.

### Define contracts and validate assets early

Write the conventions for size, axes, naming, material slots, texture color space, and expected detail tiers. Build checks for mistakes that have a visible or performance effect. Report the source asset and the violated constraint so authors can fix it without opening the runtime scene.

**Check** against intentionally mis-scaled meshes, incorrectly tagged textures, missing detail tiers, and a reimport of a valid asset. Confirm each known error is reported, valid assets still import, and the packaged build uses the intended artifacts.

### Build controls around artistic intent

Name parameters for observable properties such as wave width, wake fade distance, or silhouette highlight strength. Give each parameter units and a useful range where applicable. Keep shared defaults versioned and let specific assets override values deliberately. Provide a fast preview for iteration.

**Check** by having an artist reproduce two approved variations from the same base asset. Record the time to a usable result and inspect supported extremes for unstable output, visual discontinuity, or unexpected cost.
