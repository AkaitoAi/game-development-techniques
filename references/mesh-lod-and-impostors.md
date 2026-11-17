# Mesh LOD and impostors

## Problem and design reasoning

**Problem.** Distant objects consume geometry and shading budget, or a detail swap makes a silhouette visibly jump during normal camera movement.

**Design reasoning.** Quake III uses projected size, and Delta Force selects different distant foliage representations by platform. Both cases suggest budgeting detail according to what players can see and which operations cost the most on the device.

## Shipped examples

**Quake III Arena (1999).** The released MD3 renderer chooses among mesh LODs using a projected bounding sphere, with a configurable scale and bias. A larger or closer object tends to retain more detail, and the code selects high detail when the projection test cannot establish a usable screen size. [id Software, Quake III Arena renderer source](https://github.com/id-Software/Quake-III-Arena/blob/master/code/renderer/tr_mesh.c#L149-L208)

**Delta Force (2024).** Its developers describe using a multiview impostor as the final mesh LOD for desktop foliage, a billboard for the final LOD on mobile, and assembly cards for very distant foliage on both platforms. The choice balances triangle count, shading cost, and fidelity differently on each device. [Hang Jiao, Delta Force rendering, GDC 2025](https://media.gdcvault.com/gdc2025/Slides/Jiao_Hang_Delta%2BForce%2BPerformant.pdf)

## Techniques and practical checks

### Choose thresholds by projected error

Simplify meshes while protecting boundaries, silhouettes, attachment points, deformation, and material seams relevant to gameplay. Estimate the screen-space deviation between adjacent versions and use that or fractional screen coverage to choose a transition threshold. Keep a per-family quality bias for objects with unusual proportions or detail requirements.

**Check** a close-to-far camera rail at several fields of view and render resolutions. Compare each transition with the high-detail mesh from profile and front views. Repeat while a character animates or nearby objects move.

### Bake a distant representation to fit the available views

Use a single billboard or crossed cards for forgiving silhouettes. Use a multiview impostor when view-dependent shapes visibly change. Capture color and alpha for the intended directions and add normals or depth only when the loss without them is visible. Pad the atlas and prepare opacity-aware mipmaps.

**Check** from below, above, and across adjacent captured angles under contrasting light. Inspect silhouette wobble, atlas bleeding, depth intersections, shadow disagreement, and transparency overdraw.

### Make switching stable across frames and passes

Use separate enter and exit thresholds or a brief transition band. Make base, depth, shadow, and velocity passes agree where inconsistent coverage would produce ghosting or detached shadows. Maintain a guaranteed resident representation when higher detail streams asynchronously.

**Check** camera oscillation around each threshold, a sudden camera cut, and an artificial loading stall. Record LOD changes per second and inspect for popping, temporary holes, temporal trails, or silhouettes that disagree with their shadows.

### Compare total cost at the population scale

Budget geometry, atlas residency, draw submission, overdraw, and shadow passes. A billboard can save vertices while increasing alpha-tested pixel work. Where distant objects are seen as a group, consider a cluster proxy and set a distance where its loss of per-object parallax is acceptable.

**Check** a dense vista and a low-angle view through the same population. Compare triangle counts, GPU milliseconds, texture memory, and visual errors with and without proxies at each supported preset.
