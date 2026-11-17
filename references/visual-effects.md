# Visual effects and particles

## Problem and design reasoning

**Problem.** A flashy effect obscures a gameplay cue, becomes too expensive when several instances overlap, or takes too long to author and test. Capture both worst-case visibility and how long changes take to reach a playable build.

**Design reasoning.** Bungie documents particle tooling designed for rapid iteration and GPU simulation. An inferred trade-off is to provide a short preview loop while validating final effects against full gameplay and platform budgets. See the shipped examples below for sources.

## Shipped examples

**Resogun (2013).** Housemarque began its particle engine as a prototype for Resogun. The studio continued using the technology to stage large bursts and explosions as feedback for completing objectives and destroying enemies. [Housemarque on Resogun and Returnal](https://blog.playstation.com/?p=353670)

**Destiny 2 (2017).** Bungie's SIGGRAPH course describes particle graphs with expressions for parameters such as size and color. Bungie used a bytecode interpreter that could execute on CPU and GPU and an expression-to-HLSL converter. The architecture supported rapid editing and GPU simulation. Motion primitives let effects artists use simple shapes to influence particle movement. [Brandon Whitley, SIGGRAPH 2017 Advances in Real-Time Rendering](https://www.advances.realtimerendering.com/s2017/index.html)

**Saros (2026).** Housemarque replaced its particle engine with Graphite after supporting multiple games with the previous architecture. Its technical art director describes unifying visual-effect systems after years of features built for individual projects. The decision illustrates when a shared runtime model can be worth a migration, particularly when artists need to combine effects across systems. [Martin Contel, Housemarque](https://blog.playstation.com/?p=419776)

## Techniques and practical checks

### Drive effects from a defined gameplay event

Give an impact or movement event the location, orientation, strength and surface classification needed by the effect. Resolve hit outcomes in gameplay and use that result to select the effect. Define whether it follows an actor, remains in world space, or fades when the actor disappears.

**Check** hits on moving surfaces, pooled actors, interruptions, and scene transitions. Confirm that one outcome produces one visible response and that cosmetic quality changes preserve telegraphs.

### Scale effects by their cost and importance

Give critical cues a minimum visible representation. Scale decorative emission, active lifetime, screen coverage, and expensive distortion passes by platform and view distance. Include transparency overdraw in the budget. Large nearby sprites can dominate cost even when particle counts stay low.

**Check** a single effect close up and several simultaneous effects against the busiest background. Capture GPU time, overdraw, and active particles across presets. Verify that players can identify the cue's source and timing on the lowest preset.

### Separate preview iteration from runtime validation

Expose a small set of artist-facing parameters and provide a repeatable preview scene with the gameplay camera and lights. Save the effect asset and parameter bindings together. Compare the preview with the packaged simulation path, especially when different platforms execute different variants.

**Check** the same seeded effect across preview, packaged build, and supported simulation paths. Compare timing, bounding volume, silhouette, and event counts within defined tolerances. Measure edit-to-preview time separately from runtime frame cost.

### Use bounded environmental reaction

Use a small set of shared influences, such as wind volumes or event impulses, when nearby effects should react together. Define the region of influence and behavior when emitters leave it. Where a full field simulation is too costly, use analytic forces or sampled fields for predictable cases.

**Check** a source moving across the boundary at high speed, several overlapping impulses, and camera cuts. Inspect for discontinuities, unstable particle velocities, and lingering effects after the source is gone. Cap and measure the update work in the worst supported scene.
