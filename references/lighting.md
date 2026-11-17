# Lighting

## Problem and design reasoning

**Problem.** Static lighting becomes too costly to evaluate every frame, or important changes to lights and geometry leave the world visually inconsistent.

**Design reasoning.** Quake's baked samples and Metro Exodus's refreshed probes suggest deciding which parts of the lighting must respond during play. Allocate time and storage to the parts where responsiveness is visible, then measure what approximation the scene can tolerate.

## Shipped examples

**Quake (1996).** Offline tools traced static lights against world surfaces and stored the combined result on a coarse grid per surface. At runtime, the software renderer combined interpolated light samples with the surface texture and cached the result. Dynamic lights could add to affected lightmaps and trigger those surfaces to be rebuilt. The map data and reusable surface cache made detailed shading possible without subdividing world geometry to match every lighting change. [Michael Abrash, Quake postmortem](https://github.com/jagregory/abrash-black-book/blob/master/src/chapter-70.md)

**Metro Exodus PC Enhanced Edition (2021).** 4A Games used hardware ray tracing for diffuse global illumination and a world-space probe grid to approximate additional diffuse bounces. When refreshing the grid, the renderer reads the previous frame’s probe data, allowing the contribution of successive bounces to accumulate across frames. Moving or changing illumination can therefore feed the indirect lighting system, subject to its sampling and update budget. [4A Games technical breakdown](https://www.4a-games.com.mt/4a-dna/in-depth-technical-dive-into-metro-exodus-pc-enhanced-edition)

**What changed.** The historical example reuses lighting computed principally at build time. The newer example caches part of the solution during play and refreshes it as conditions change. Build-time lighting remains a good fit for stable level geometry and predictable run-time cost. Dynamic indirect lighting earns its cost when moving lights, changing time of day, or changing geometry visibly affect play. Sparse caches trade update time and memory against spatial detail and responsiveness.

## Techniques to try

| Technique | Implementation | Verification |
|---|---|---|
| Bake stationary lighting | Give static meshes a dedicated nonoverlapping lightmap unwrap with padded islands. Allocate more texels to close-up surfaces and sharp baked shadow boundaries. Keep unbaked moving geometry on a separate lighting path. | Build a room with a doorway and a thin partition. Inspect the floor-to-wall seams and both sides of the partition at gameplay exposure. Compare lightmap memory and artifacts across two texel densities. |
| Light dynamic objects from probes | Sample interpolated diffuse probes for character ambient light, with direct lighting and shadows evaluated separately. Place probes around transitions and use visibility-aware interpolation or separate probe regions where thin walls divide differently lit spaces. | Walk a neutral sphere from a bright room into a dark room. Look for sudden changes at cell boundaries and light leaking through the wall. |
| Budget live shadows | Rank lights by gameplay importance and screen coverage. Reuse static shadow results where supported and reduce shadow range or update frequency on secondary lights. Invalidate cached coverage when its light or casters change. | Follow a moving caster across a cache boundary and toggle a light. Check that shadows update within the project’s latency target, then measure the shadow pass in a scene with many overlapping lights. |
| Test dynamic indirect lighting under change | Limit probe updates or ray count to a measured budget and make exceptional changes such as teleports or major lighting transitions invalidate stale history. | Record a door opening between a sunlit and dark room and an emissive source switching off. Check dark-room leakage, temporal lag, ghosting, and indirect-light GPU time on target hardware. |

For a compact lighting test level, include a thin wall, a bright doorway, a moving character, and a toggled light. Keep exposure fixed for comparisons, then repeat with the project’s shipping exposure settings. Inspect both stills and video since an image cannot reveal temporal flicker or delayed updates.
