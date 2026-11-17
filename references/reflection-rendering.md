# Hybrid reflection rendering

## Problem and design reasoning

Reflections need information about surfaces beyond the camera's view. Screen-space reflections can show moving objects the scene already rendered, but lose data at screen edges and behind occluders. Captured environment maps offer stable coverage, but cannot faithfully track every local change or reflection origin. Combine sources based on validity, roughness, and budget so an invalid sample has a predictable replacement.

## Shipped examples

**Killzone Shadow Fall (2013).** Guerrilla combined screen-space ray marching with artist-placed local cubemap zones and a distant environment cubemap. When a screen-space hit was unavailable, the system used the local and then background data. Its filtering accounted for material roughness. [Michal Valient, The Production and Visual FX of Killzone Shadow Fall, GDC 2014](https://www.guerrilla-games.com/media/News/Files/GDC2014_Valient_Killzone_Graphics.pdf)

**Need for Speed Heat (2019).** Ghost Games and Frostbite reported that no single reflection technique covered every use case. The renderer let artists select reflection systems for objects and used a continuously updated cubemap centered on the player car, static volumes for the world, stochastic screen-space reflections, and a planar fallback. **Design inference.** A moving cubemap is easiest to justify when the player spends most of the game near one prominent reflective subject. [Garcia and colleagues, Hustle by Day, Risk it all at Night, SIGGRAPH 2020](https://history.siggraph.org/wp-content/uploads/2022/08/2020-Talk-Garcia_Hustle-by-Day-Risk-it-all-at-Night.pdf)

## Techniques and practical checks

### Define confidence for each screen-space hit

Treat out-of-bounds rays, missing geometry, silhouette crossings, and unstable depth matches as low-confidence results. Expose confidence as a debug layer so the fallback blend can be inspected independently of the final image.

**Check** pan a bright moving object across a screen edge while its reflection remains in view. Look for a smooth transition to a fallback without detaching or stretching the object along its last visible pixels.

### Correct local probe parallax

Provide capture origins and room-scale proxy geometry. For a box proxy, use the shading point and reflection direction to find a proxy hit, then sample the cubemap in the direction from the capture origin to that hit. Blend overlapping volumes without letting distant probes dominate nearby interiors.

**Check** move parallel to a polished wall or floor in a rectangular room. Compare captured wall corners with the expected reflected positions and observe continuity across neighboring probe influences.

### Keep roughness response consistent across sources

Prefilter captures for the renderer's material response and match the apparent blur of each reflection technique across the roughness range. Use confidence and material response to blend sources while preserving reasonable brightness when quality settings change.

**Check** render a row of materials from mirror-smooth to rough under fixed exposure. Compare how brightness and roughness change when each reflection path is enabled individually and when fallback blending is active.

### Spend dynamic work near important surfaces

Reserve planar captures for influential surfaces or bounded viewpoints, and update dynamic cubemaps according to movement, lighting changes, or scene events. Measure the whole capture cost, including geometry and post-processing used in additional views.

**Check** compare a static scene with a fast camera or vehicle route and moving lights. Capture GPU timings, visible update lag, and quality-tier behavior. Temporarily disable each source to confirm it improves the intended surfaces.
