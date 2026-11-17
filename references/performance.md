# Performance optimization

## Problem and design reasoning

**Problem.** Frame spikes or crashes occur in specific scenes, while averages or isolated benchmarks conceal the reason.

**Design reasoning.** Naughty Dog connected frame measurements to the scene, and Epic changed its path for objects that actually need deformation. The design inference is to profile representative workloads and avoid paying expensive costs when the effect is inactive.

## Shipped examples

**Doom (1993).** In the released Linux Doom renderer, the binary space partition traversal visits the viewer-side subtree first. Before visiting the far subtree, it checks whether the subtree's projected bounds fall outside the view or inside a screen span already blocked by solid walls. The test can skip a whole region before walking its walls and sprites. [id Software renderer source](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/r_bsp.c#L373-L541)

**Uncharted 2: Among Thieves (2009).** Naughty Dog’s postmortem shows an in-game profiler timeline with separate lanes for PlayStation 3’s SPUs, main processor threads, and GPU. Its captures pair frame timings with location and rendering counters, including visibility passes and renderable counts. The profiling approach lets developers relate a slow encounter to both the processor doing the work and the scene conditions producing it. [Richard Lemarchand, GDC 2010 postmortem](https://media.gdcvault.com/gdc10/slides/Lemarchand_Richard_Uncharted2_Postmortem_PLEASE-TURN-ON-COMMENTS-LAYER.pdf)

**Fortnite Battle Royale Chapter 4 (2022).** Building meshes could wobble when damaged. A zero animation weight still left the vertex displacement evaluation on the programmable rasterization path. Epic made the game enable displacement evaluation while a building piece was wobbling, allowing idle pieces to use the fixed-function path. Epic also reported that opaque foliage with explicit geometry was usually faster for this content than masked foliage cards, whose overlapping transparent areas contributed to overdraw. [Epic Games technical breakdown](https://www.unrealengine.com/tech-blog/bringing-nanite-to-fortnite-battle-royale-in-chapter-4) · [Fortnite Chapter 4 launch notes](https://www.fortnite.com/news/drop-into-the-next-generation-of-fortnite-battle-royale-powered-by-unreal-engine-5-1)

**What changed.** The cases range from skipping hidden regions in a software renderer, through profiling a fixed console’s heterogeneous processors, to selecting GPU rasterization paths based on gameplay state. A visibility test still pays when its cost is lower than the work it avoids. Use representative scenes to measure that tradeoff and locate the dependency, processor, or pass that determines frame completion.

## Techniques to try

| Technique | Implementation | Verification |
|---|---|---|
| Capture frame time by subsystem | Set the frame budget from the release target, such as 16.67 ms at 60 Hz. Capture CPU and GPU timelines with named scopes for simulation, submission, rendering, and asset streaming. Record a deterministic route through play with both cold and warm caches. | On the slowest supported hardware, report median, p95, p99, and the worst spikes in milliseconds. Inspect waits and overlapping CPU/GPU work in the timeline before changing code. |
| Skip inactive work | Gate optional work on the gameplay state that makes it observable. Avoid scanning all inactive objects for state changes when existing events can drive activation. Keep any required wake-up or state restoration on reactivation. | Compare idle-heavy, active-heavy, and rapidly alternating cases. Verify identical activation timing and output, then check whether the total frame time improves with the intended workload. |
| Reduce work by visibility or distance | Use conservative visibility bounds, screen-size-based detail levels, and hysteresis where assets switch tiers. Include animation motion in bounds where it affects visibility. Beware of batching so much geometry together that hidden pieces can no longer be culled independently. | Run a fast camera route and a slow sweep past boundaries. Review popping, missing geometry, CPU submission cost, GPU pass time, and visible primitive counts. |
| Control asset residency and uploads | Prefetch likely next assets, cap new uploads per frame, and retain high-use assets within a platform-specific memory budget. Give critical assets precedence over decorative assets when their requests compete. | Rapidly traverse a streaming boundary on a cold launch, then reverse direction repeatedly. Track peak memory, upload stalls, missing assets, and p99 frame time. |

Record the build, device, quality preset, resolution, and test route with each capture. Keep the scenario fixed when comparing a change, then run an adverse case where its assumptions break, such as a dense fight or rapid traversal. If a change only improves a pass that is hidden behind other work, check whether it creates useful headroom or lowers the number of missed frame deadlines.
