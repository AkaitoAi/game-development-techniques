# Light probes and irradiance

## Problem and design reasoning

Movable surfaces need coherent diffuse bounce lighting as they cross rooms, stand near a bright opening, or change orientation. Sampling indirect lighting throughout space lets objects use the same field without per-object lightmap UVs. Sparse spatial samples can still leak through walls or flatten the difference between the lit and shaded ends of an object. Directional representations add normal-dependent response, but their angular bandwidth limits how sharply they reproduce concentrated light.

## Shipped examples

**Quantum Break (2016).** Remedy used irradiance volumes augmented with precomputed sun and sky transport. The talk identifies support for dynamic objects, alternate mesh detail levels, and volumetric lighting as advantages of this shared representation. The team handled specular reflections through a separate path because representing sharp angular detail in the transport data would increase storage substantially. [Remedy, Multi-Scale Global Illumination in Quantum Break, SIGGRAPH 2015](https://www.advances.realtimerendering.com/s2015/SIGGRAPH_2015_Remedy_Notes.pdf)

**The Last of Us Part II (2020).** Naughty Dog found that interpolating ambient probes once at the object center missed spatial variation. The team instead evaluated lighting at the eight corners of an oriented bounding box, stored results in an atlas, and interpolated at shaded pixels. It reused cached results for unchanged objects and baked directional occlusion information to limit leakage. **Design inference.** Bounding-box sampling fits objects with moderate extent. Very long or irregular meshes may need a denser lookup or partitions. [Naughty Dog, Lighting Technology of The Last of Us Part II, SIGGRAPH 2020](https://history.siggraph.org/wp-content/uploads/2022/08/2020-Talks-Doghramachi_Lighting-Technology-of-The-Last-of-Us-Part-II.pdf)

## Techniques and practical checks

### Validate directional evaluation in isolation

Evaluate a low-order spherical harmonic representation against the shading normal in a known coordinate space. Confirm whether coefficients already encode cosine-convolved irradiance so the shading implementation applies the diffuse response consistently.

**Check** a neutral sphere under constant ambient lighting and under a bright off-axis source. Turn off direct lighting. Compare shading as the sphere rotates and inspect opposite-facing normals for ringing or negative values.

### Allocate samples around lighting transitions

Use a coarser grid in homogeneous space and refine near doors, tight corridors, and transitions between shade and sunlight. Add a visibility test or bake occlusion information to reduce interpolation across solid geometry. Keep a defined fallback for regions without usable samples.

**Check** walk a neutral test object across connected rooms and past an adjacent room separated by a thin wall. Visualize selected probes and weights. Compare leakage and popping before and after changing spacing or visibility.

### Select the right spatial evaluation rate

Choose per-object, per-vertex, or per-pixel evaluation based on object size, expected irradiance variation, and frame budget. If evaluating a small volume per object, tie cache invalidation to changes in bounds, movement, and lighting data.

**Check** send a long object through a lit doorway, then leave it stationary while changing the loaded probe data. Verify the lit and shaded ends remain distinct and cached lighting refreshes when its source changes.

### Budget runtime updates and streaming

Track probe density, coefficient precision, residency, and upload costs. Establish rebuild priorities for boundaries where players approach unloaded volumes, and retain a coarse representation until finer data arrives.

**Check** repeat the same route through two streamed areas while turning the camera toward affected geometry. Inspect transitions for one-frame black samples, false brightness steps, peak memory, and frame-time spikes.
