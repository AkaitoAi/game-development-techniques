# Terrain and vegetation

## Problem and design reasoning

**Problem.** A landscape looks repetitive, vegetation movement lacks coherence, or dense foliage disproportionately raises GPU cost. Inspect worst-case views as well as quiet scenic areas.

**Design reasoning.** Crytek's documented shared wind signal and asset-specific responses retain variation under consistent motion. Ubisoft's ray-tracing example illustrates the inferred rule of validating acceptable approximations against a reference under representative coverage. See the shipped examples below for sources.

## Shipped examples

**Crysis (2007).** Crytek animated vegetation with main bending driven by wind and separate detail bending for fine movement. Vertex color channels supplied artist-authored controls for local behavior. A shared wind signal and local response parameters let different plant assets move together while retaining their own characteristics. [Tiago Sousa, Crytek, GPU Gems 3](https://developer.nvidia.com/gpugems/gpugems3/part-iii-rendering/chapter-16-vegetation-procedural-animation-and-shading-crysis)

**Ghost of Tsushima (2020).** Sucker Punch generated individual grass blades on the GPU so each could vary in appearance and motion. The developer presentation describes generating grass from tiled terrain data. Wind also serves as a navigation cue, so the environment's movement carries gameplay information. [Eric Wooley, GDC Advanced Graphics Summit](https://gdcvault.com/play/1027214/) · [Sucker Punch developer write-up](https://blog.playstation.com/?p=345372)

**Assassin's Creed Shadows (2025).** Ubisoft's SIGGRAPH slides describe a cheaper opacity approximation for ray-tracing intersections in scenes with vegetation. Scaling geometry according to average opacity and treating intersections as opaque reduced trace cost on the tested hardware. The team compared the approximation visually with an alpha-tested reference. [Ubisoft, SIGGRAPH 2025 Advances in Real-Time Rendering](https://advances.realtimerendering.com/s2025/content/Advances%202025%20-%20Raytracing%20the%20world%20of%20Assassin%27s%20Creed%20Shadows.pdf#page=85)

## Techniques and practical checks

### Keep tiled terrain consistent across representations

Use a shared coordinate grid and an explicit height convention. Share border samples between neighboring tiles and choose a stitching method, such as compatible edge tessellation or indexed stitch strips, for tiles that meet at different resolutions. Preserve a collision representation with the accuracy required for supported movement. Use supplemental meshes for overhangs or caves when a heightfield cannot represent them.

**Check** adjacent tiles at every supported detail pairing, with low-angle views and during fast traversal. Measure border gaps and grounding height differences. Include large-coordinate positions and a location where terrain meets an independently authored mesh.

### Constrain and layer vegetation placement

Use height, slope, biome, and material masks to control where each species can appear. Sample deterministic candidate positions in world coordinates. Use one rule for ownership along tile boundaries, and evaluate neighbor constraints across those boundaries. Provide exclusion and override layers for paths, entrances, and authored sightlines.

**Check** a biome boundary, a steep slope, and a road crossing two tiles. Regenerate tiles in different orders and inspect for duplicated plants, gaps, seed changes, and blocked traversal or signs.

### Build vegetation motion from shared signals

Sample a world-space wind field and vary response by species, bend weight, and instance phase. Anchor the base of each plant and reserve finer motion for tips or leaves. Use small local fields or bend impulses for nearby movement when the interaction is visible.

**Check** a camera cut and crossing multiple tiles during a gust or directional cue. Look for motion restarting, detached roots, and inconsistent wind direction. Check the effect under reduced-motion settings where supported.

### Pick detail tiers by measured screen and GPU cost

Use separate tiers for silhouette, shadow, and collision when they serve different needs. Group rendering work spatially but keep groups small enough for useful culling. Compare meshes, instanced blades, masked cards, or impostors with the camera ranges and lighting the game actually uses. Keep consequential cover and collision stable when render tiers change.

**Check** a slow sweep and a rapid approach through dense vegetation. Record frame time, overdraw, shadow cost, and visual popping. Repeat at the lowest preset with any navigation, stealth, and interaction cues that depend on the plants.
