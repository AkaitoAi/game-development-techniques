# Ambient occlusion

## Problem and design reasoning

Indirect light from a uniform environment makes wall junctions, contacts, and small material cavities look flatter when the lighting representation misses nearby blockers. Ambient occlusion estimates how much of the local hemisphere is visible. Choose an estimator for the detail scale and the geometry available to sample, then account for occlusion already present in the indirect-light solution.

## Shipped examples

- **Call of Duty: WWII.** Sledgehammer's SIGGRAPH 2018 course documents an offline material conversion step that reconstructs a height map from the authored normal map and processes it with GTAO to estimate material-scale occlusion. The authors also describe combining material and screen-space visibility using the minimum in one tessellation path and acknowledge that the estimates can represent different scales. This illustrates a practical compromise when the available signals partly overlap. [SIGGRAPH 2018 course notes, sections 4.1–4.7](https://advances.realtimerendering.com/s2018/MaterialAdvancesInWWII-course_notes.pdf)
- **Assassin's Creed Shadows.** Ubisoft's SIGGRAPH 2025 talk documents an irradiance-probe correction for features shorter than the probe spacing, ray-traced ambient occlusion, and a GTAO pass tuned to recover detail lost through filtering or absent from the ray-tracing acceleration structure. The transferable inference is to compare each estimator against the source of missing detail before increasing its intensity. [SIGGRAPH 2025 presentation, Occlusion section](https://advances.realtimerendering.com/s2025/content/Advances%202025%20-%20Raytracing%20the%20world%20of%20Assassin%27s%20Creed%20Shadows.pdf)

## Techniques and practical checks

- **Separate material cavities from scene contacts.** Store material occlusion in surface data when detail is below the mesh scale. Estimate movable contacts from the available scene representation. **Check** a patterned surface on an isolated plane, then put a movable object on it. Verify that the surface pattern follows the material and the contact changes with the object's position.
- **Tune radius in scene units.** Choose a radius based on the intended physical scale, then account for depth projection and cap the sample footprint. **Check** the same corner at near and far camera distances and at different resolutions. Inspect the occlusion buffer for inconsistent apparent radius and halos along depth discontinuities.
- **Control temporal errors.** Reject history across depth or normal discontinuities and invalidate it after camera cuts. Use reprojection and filtering suitable for the available motion vectors. **Check** a moving character and a camera sweep that reveals a previously hidden wall. Look for trails, edge shimmer, and occlusion that persists after its blocker leaves.
- **Inspect composition and cost.** Expose the separate lighting and occlusion terms for comparison, and apply visibility to the intended indirect contribution. **Check** each term in isolation and together using a lit corner, an unoccluded surface, and a side-lit scene. Look for double darkening and profile the pass on minimum-spec hardware.
