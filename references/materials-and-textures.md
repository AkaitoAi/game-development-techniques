# Materials and texture pipelines

## Problem and design reasoning

**Problem.** Similar surfaces respond inconsistently, textures shimmer at distance, or terrain material detail consumes more memory than the platform can afford.

**Design reasoning.** Ready At Dawn used shared material inheritance and compositing to support consistent authoring, while Quake loaded prepared mip levels. These examples favor encoding repeatable surface decisions in assets and tooling where authors can inspect and reuse them.

## Shipped examples

**Quake (1996).** The released WinQuake source loads world textures with stored offsets for each mip level. Preparing lower-resolution versions as part of the asset lets rendering sample content appropriate to its projected size. [id Software, world texture loader](https://github.com/id-Software/Quake/blob/master/WinQuake/model.c#L338-L384)

**The Order: 1886 (2015).** Ready At Dawn combined physically based shading with material inheritance and compositing. Its pipeline covered scan acquisition, offline compositing, and runtime layer blending. The shared material definitions aimed to speed up authoring and keep material response consistent across production content. [David Neubelt and Matt Pettineo, GDC 2014](https://www.gdcvault.com/play/1020485/Crafting-a-Next-Gen-Material)

**Call of Duty: Black Ops Cold War (2020).** Treyarch's terrain talk describes using virtual texturing for seamless blending. Activision's follow-up course describes its broader Call of Duty terrain system in terms of pages representing square regions of the virtual texture and its mips. This extends mip selection into a residency problem because portions of a texture can become available independently. [JT Hooker, GDC 2021](https://www.gdcvault.com/play/1027463) · [Activision, SIGGRAPH 2023](https://advances.realtimerendering.com/s2023/Etienne%28ATVI%29-Large%20Scale%20Terrain%20Rendering%20with%20notes%20%28Advances%202023%29.pdf)

## Techniques and practical checks

### Give every map explicit semantics

Define whether a texture stores display-encoded color or linear data. Decode authored color according to its encoding before shading. Sample normals, roughness, metallic values, and masks as linear data and document normal-map orientation, channel packing, and value ranges. Keep an import preset for each role so asset changes use consistent settings.

**Check** a known mid-gray color ramp, a roughness ramp, and a normal map with an obvious directional bump. Compare shader inputs in a frame capture under a known light. Mirror a UV island and verify the tangent basis keeps the highlight facing the intended direction.

### Author reusable material layers

Define named base materials for recurring surfaces. Compose overlays with explicit mask and blend rules. Bake layers when their parameters are fixed. Keep a layer live when weather, interaction, or damage must change it during play. Measure the texture reads and shader work added by each live layer.

**Check** shared wood and metal materials under warm, cool, and neutral lighting, then apply them to multiple assets. Change one base parameter and confirm intended propagation. Inspect an overlapping mask boundary up close and at a distance, and profile the busiest layered view.

### Cook mips and compression by texture role

Generate color mips from linear-light values, filter normals with direction in mind, and consider coverage-preserving mips for alpha-tested silhouettes. Choose compression according to channels, fidelity, and supported hardware. Leave a fallback for formats unsupported on a given target. Review generated data as part of import.

**Check** a shallow-angle surface, distant foliage, a smooth gradient, and a high-frequency normal map in motion. Compare cooked and uncompressed versions for color shifts, block artifacts, flicker, and silhouette loss. Record texture bytes and frame time on the lowest supported device.

### Set a texture residency budget

Assign coarse mips and fine detail separate priorities. Reserve capacity for visible and gameplay-critical assets, use measured lead time for prefetch, and limit uploads per frame. Use tiled virtual textures where the workload benefits from loading only selected regions. Test fallback sampling at tile and mip boundaries.

**Check** a cold start, a fast traversal, a sudden close-up, and a return to an evicted area. Log peak resident memory, upload spikes, missing tiles, and how long coarse detail remains visible. Confirm readable decals and interactive cues throughout.

### Validate UV density and padding before painting

Apply a numbered checker while laying out UV islands. Match texel density across adjacent surfaces unless viewing distance or visual priority justifies a different density. Flag intentional overlaps and keep unique geometry marks or bakes on unique islands. Build gutters from extended island colors so that filtering into lower mips samples a compatible border. [Blender Foundation's render-baking reference](https://docs.blender.org/manual/en/latest/render/cycles/baking.html)

**Check** the checker at the asset's intended screen size and across every important seam. Swap in the baked and painted textures, inspect several cooked mip levels, and look for bleed between islands. Repeat with mirrored islands and a high-contrast neighboring texture.
