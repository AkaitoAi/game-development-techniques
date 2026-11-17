# Volumetric effects

## Problem and design reasoning

Fog cards can represent wisps, but their intersecting planes and compositing order become visible when cameras enter thick haze or dynamic lights move through it. A participating-medium model gives light, density, and depth a shared representation. Its spatial sampling and temporal history trade GPU cost and stability against resolution and responsiveness.

Naughty Dog documented a new volumetric system to blend several fog environments with the world. Housemarque documented the limitations of using heavily filtered low-frequency fog for reactive detail in Returnal and adopted separate low- and high-frequency systems in Saros. These accounts support a design inference: choose the representation per effect scale and response time, and validate each under camera and light movement. [Artem Kovalovs, Volumetric Effects of The Last of Us: Part Two, SIGGRAPH 2020](https://history.siggraph.org/wp-content/uploads/2022/08/2020-Talks-Kovalovs_Volumetric-Effects-of-The-Last-of-Us-Part-Two.pdf) · [Housemarque, The VFX tech behind Saros](https://blog.playstation.com/2026/06/17/the-vfx-tech-behind-saros-from-ngp-to-graphite/)

## Shipped-game examples

- **The Last of Us Part II.** Naughty Dog's SIGGRAPH presentation describes a volumetric system built to support different fog environments and tie them into the rest of the scene. [Artem Kovalovs, SIGGRAPH 2020](https://www.naughtydog.com/blog/naughty_dog_at_siggraph_2020)
- **Saros.** Housemarque uses low-frequency fog for ambient atmosphere and high-frequency fog for character effects in selected rooms. The team also documents reducing history weight in the coarse fog and using sampling and depth adjustments to manage aliasing. [Housemarque engineering breakdown](https://blog.playstation.com/2026/06/17/the-vfx-tech-behind-saros-from-ngp-to-graphite/)

## Techniques and practical checks

- **Validate extinction in a uniform medium.** Specify extinction in inverse world-distance units. Over a segment of length `d` with constant extinction `sigmaT`, transmittance is `exp(-sigmaT * d)`. Reduce transmitted scene light and add integrated in-scattered light along the view ray. **Check** a uniform unlit volume at several known depths against the analytic transmittance. Change step count and confirm that the result converges without changing the medium's apparent scale.
- **Separate ambient haze from local detail.** Use a coarse volume for broad illumination and reserve finer sampling or local effects for fast, close, or visually important structure. Define how overlapping densities and extinction combine. **Check** a local dust plume moving through ambient haze near a wall. View from inside and outside, and inspect the boundary for a visible seam, intensity discontinuity, or double darkening.
- **Handle history invalidation deliberately.** Reproject when the previous sample represents the same part of the medium, limit accumulation at disocclusions, and clear history on camera cuts. Tune history weight against the slowest light or fog transition that must remain readable. **Check** a stationary view, rapid pan, moving light, and teleport under a reproducible camera path. Record flicker and trailing separately.
- **Choose quality tiers using representative shots.** Measure volume resolution, view range, lights, integration steps, and compositing cost. Preserve the geometry behind translucent or opaque boundaries through depth-aware composition. **Check** a near silhouette, long view, overlapping local lights, and a thin foreground occluder at each tier. Compare GPU frame times, light leaks, and loss of gameplay contrast.
