# Shadow rendering

## Shipped examples

- **Doom 3 (2004).** The released source constructs stencil shadow volumes for dynamic lights. The GDC presentation on robust shadow volumes explains the stencil test and the geometry needed to preserve correct results when volumes intersect the camera's near plane. [id Software source](https://github.com/id-Software/DOOM-3/blob/master/neo/renderer/tr_stencilshadow.cpp) · [GDC 2002, Robust Shadow Volumes](https://developer.download.nvidia.com/assets/gamedev/docs/GDC2002_RobustShadowVolumes.pdf)
- **Assassin's Creed Shadows (2025).** Ubisoft's SIGGRAPH course describes the title's ray tracing implementation and scalability choices, including virtualized geometry used for ray-traced effects. [Ubisoft, SIGGRAPH 2025, Raytracing the World of Assassin's Creed Shadows](https://advances.realtimerendering.com/s2025/content/Advances%202025%20-%20Raytracing%20the%20world%20of%20Assassin%27s%20Creed%20Shadows.pdf)

## Problem and design reasoning

A light needs visibility information for each receiver. Shadow volumes represent the occluded region geometrically. Shadow maps sample depths from the light's view and trade accuracy against texture resolution, bias, and filter cost. Ray-traced visibility adds a representation and traversal budget for scene geometry. The inference for engine selection is to test against the lights, moving casters, and hardware that the game actually ships, since each representation fails in different scenes.

## Techniques and practical checks

### Assign a budget per light

Classify lights by screen influence, movement, and gameplay importance. Allocate shadow resolution, update frequency, and maximum range accordingly. Reuse a cached static contribution only while the light and its cached casters remain valid.

**Check** move a caster through a scene with several overlapping lights. Toggle a light and move a previously static caster. Measure cost and verify that no stale shadow persists.

### Tune bias and filtering against contact

Use depth or normal bias in units appropriate to the projection and filtering footprint. Adjust alongside resolution. A wider filter can hide stepped edges while increasing apparent separation from contact if bias is excessive.

**Check** compare a thin railing, a sloped surface, and an object touching the ground with a grazing light. Record self-shadowing artifacts and detached contact edges at close and far viewing distances.

### Stabilize directional coverage

Use tighter coverage near the camera and lower detail farther away. If cascading, stabilize the light-space projection against small camera motions and blend coverage at split boundaries.

**Check** pan slowly across a split, then rotate abruptly while holding the light fixed. Record edge shimmer, transition seams, atlas occupancy, and the time each partition adds.

### Verify caster parity across techniques

Define how alpha cutouts, animated meshes, and deforming vegetation appear to each shadow technique. Set a fallback for unsupported devices and measure it at the same gameplay locations.

**Check** compare moving foliage, an animated silhouette, and a thin cutout across quality levels. Include disocclusion after a camera cut and inspect contact, lag, and GPU frame time.
