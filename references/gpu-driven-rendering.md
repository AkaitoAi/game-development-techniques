# GPU-driven rendering

## Problem and design reasoning

**Problem.** CPU submission work grows with a view full of repeated objects, even when most candidates are hidden or too small to matter.

**Design reasoning.** Ubisoft's hierarchical GPU visibility tests suggest a preference for cheap early rejection before detailed work. Moving selection closer to submission is worthwhile when the candidate volume pays for the additional GPU work and buffers.

## Shipped examples

**Assassin’s Creed Unity (2014).** Ubisoft’s renderer handled modular buildings and crowds using GPU instance and cluster culling, index compaction, and multidraw submission. Its hierarchy allows inexpensive tests to discard groups before finer tests examine their geometry. [Ulrich Haar and Sebastian Aaltonen, GPU-Driven Rendering Pipelines, SIGGRAPH 2015](https://advances.realtimerendering.com/s2015/aaltonenhaar_siggraph2015_combined_final_footer_220dpi.pdf)

**Ghost of Tsushima (2020).** Sucker Punch had to process millions of authored world instances on PS4. Its grass renderer generates individual blades on the GPU and gives them procedural appearance and motion. This is a different use of GPU-side work, where compact inputs describe fine detail that the renderer expands near the view. [Matthew Pohlmann, Samurai Landscapes, GDC 2021](https://gdcvault.com/play/1027352/Samurai-Landscapes-Building-and-Rendering) · [Eric Wohllaib, Procedural Grass in Ghost of Tsushima, GDC 2021](https://gdcvault.com/play/1027214/)

## Techniques and practical checks

### Keep a stable instance table

Store transforms, conservative bounds, and references to geometry and materials in a GPU-readable table. Update changed records in batches. Delay reuse of freed entries until any frames still referencing them have completed. First move visibility and command generation for one large, coherent category such as opaque static instances.

**Check** a fixture that inserts and removes thousands of instances while several frames are in flight. Compare rendered IDs with a CPU reference and record CPU submission time, bytes uploaded, and maximum table occupancy.

### Cull hierarchically with conservative bounds

Test coarse instance or cell bounds against the view before expanding mesh clusters or procedural geometry. Where supported, use a hierarchical depth buffer for occlusion queries. Keep bounds large enough to contain animation and vertex displacement. Define when occlusion history is invalid and allow uncertain candidates through.

**Check** a camera cut, rapid turn, near-plane crossing, a moving occluder, and displaced foliage. Compare against a frustum-only capture for incorrectly missing geometry. Log survivors by culling stage instead of judging only from the final draw count.

### Compact survivors into compatible batches

Group visible candidates by the geometry and pipeline state your backend can submit together. A compute pass can write bounded lists of instances and fill indirect draw arguments. Declare the dependency between buffer writes and command reads, and provide an observable overflow policy.

**Check** a forced small buffer limit and a frame with every candidate visible. Confirm that commands stay within valid storage, that overflow is reported, and that the defined recovery path preserves required objects. Exercise multiple passes and frames in flight under graphics validation.

### Measure the cost transferred to the GPU

Capture culling, compaction, indirect execution, and drawing separately. Compare the full frame with a CPU-submitted baseline at similar visual quality. Retain a simpler path where the required GPU features are unavailable or the scene contains too little work to repay the dispatch costs.

**Check** a dense vista, a mostly occluded interior, and a sparse scene on representative devices. Track CPU and GPU frame-time tails, transient memory, and output parity between paths.
