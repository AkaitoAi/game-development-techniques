# Rendering pipelines

## Problem and design reasoning

**Problem.** A frame exceeds budget only in scenes with certain material, light, or geometry combinations. Attribute GPU time to passes and identify the content feeding each bottleneck.

**Design reasoning.** Bungie documents a hybrid deferred and forward pipeline in Halo: Reach. The inferred design principle is to route content by its actual costs and shading requirements, with a measured boundary between paths. See the shipped examples below for sources.

## Shipped examples

**Halo: Reach (2010).** Bungie documented a hybrid deferred renderer. It stored a compact set of material attributes for deferred local lighting, while a forward path handled some vertex-heavy geometry, including foliage, to limit repeated vertex processing. A single frame can use several shading paths when geometry costs, material requirements, and light overlap differ. [Natalya Tatarchuk, SIGGRAPH 2013 Advances in Real-Time Rendering](https://advances.realtimerendering.com/s2013/Tatarchuk-Destiny-SIGGRAPH2013.pdf)

**Fortnite Battle Royale Chapter 4 (2022).** Epic describes adapting its virtual geometry path for Fortnite's buildings, foliage, and changing geometry. Programmable rasterization let selected materials use features such as masking and vertex displacement. Epic could switch displacement evaluation off for idle pieces. The pipeline selection became part of asset authoring and gameplay integration. [Epic Games developer breakdown](https://www.unrealengine.com/tech-blog/bringing-nanite-to-fortnite-battle-royale-in-chapter-4)

## Techniques and practical checks

### Select shading paths by scene cost

Prototype forward shading for scenes with few overlapping lights or where material flexibility and multisampling matter. Compare deferred shading where many opaque pixels receive many local lights. Include a separate transparency strategy when surfaces need order-dependent blending. Record the costs of material evaluation, geometry submission, intermediate buffers, and resolving samples.

**Check** a low-light scene, a dense light volume scene, and foliage or glass under motion. Measure frame time and peak render-target memory at target resolutions. Compare silhouettes, decals, and transparency against the expected result.

### Declare resource dependencies and lifetimes

For each pass, declare the resource versions it reads and writes, their expected format and dimensions, and queue ownership where queues overlap. Schedule passes from dependencies and keep imported targets and cross-frame history explicit. Reuse transient memory when the backend confirms compatible resources have disjoint GPU lifetimes. [Yuriy O’Donnell, Frostbite FrameGraph, GDC 2017](https://www.slideshare.net/slideshow/framegraph-extensible-rendering-architecture-in-frostbite/72795495)

**Check** with a graphics debugger and API validation. Toggle optional passes, resize the viewport, and compare runs with and without memory aliasing. Confirm correct output and compare memory high-water marks. Rerun with async work enabled where supported.

### Keep frame stages coherent under configuration changes

Decide which passes share depth, normals, velocity, and object visibility. Define expected behavior for deforming meshes, cutout surfaces, and camera-relative effects in depth, shadow, and color passes. Explicitly reset temporal history on cuts, invalid resource changes, and other discontinuities.

**Check** rapid camera cuts and resolution changes against animated foliage or characters. Inspect intermediate targets for silhouette disagreement, missing velocity, and stale history. Repeat at every supported quality tier.

### Add per-pass observability

Label render passes and expose key intermediate images. Capture representative frames with GPU timings, resource lifetimes, and draw or dispatch counts. Give expensive features a visual fallback so they can be toggled while measuring the bottleneck.

**Check** a known occluded mesh, an alpha-tested surface, and overlapping shadow casters. Inspect pass output and timing when disabling each applicable feature. Investigate timing shifts on other passes before concluding where work was saved.
