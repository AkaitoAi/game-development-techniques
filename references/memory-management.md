# Memory management and allocation

## Problem and design reasoning

**Problem.** Memory rises after repeated scene transitions, or an allocation stalls a busy frame despite acceptable average usage.

**Design reasoning.** Doom's reclaimable tags and Capcom's allocator partitioning express different deadlines and lifetimes. The design inference is to assign memory ownership and reclamation based on the work and validate its highest-pressure moments.

## Shipped examples

**Doom (1993).** id Software's released Linux code tags blocks within its zone allocator. It can release ranges of tags, reclaim purgeable allocations as it searches for space, and merge neighboring free blocks. The caller can distinguish allocations with different lifetimes and reclamation rules. [id Software, zone allocator](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/z_zone.c)

**Street Fighter 6 and Exoprimal (2023).** Capcom's allocator talk identifies these games as shipped users of its virtual-memory allocator. It describes segmenting the budget by use and taking different approaches for performance-critical workers and lower-priority work. The segmentation gives developers a view of consumption by purpose. [Capcom Open Conference Professional RE:2023](https://www.docswell.com/s/CAPCOM_RandD/ZXYDVM-RE2023)

**Fortnite Creative.** Epic's conference material explains the pressure of fitting a single experience into the memory limits of supported devices. Epic's documentation distinguishes the editor's content-based memory calculation from measurements of a running session. Validate both what content could be resident and what actually becomes resident under load. [Unreal Fest 2023](https://www.youtube.com/watch?v=gtX0gPOSkbU) · [Epic memory management documentation](https://dev.epicgames.com/documentation/fortnite/memory-management-in-unreal-editor-for-fortnite)

## Techniques and practical checks

### Budget by lifetime and owner

Track persistent game state, scene storage, frame scratch, and cached assets independently. Choose budgets on target hardware and include headroom for transient duplication during transitions. Track high-water marks for the metric that constrains your platform, such as committed memory, physical residency, video memory, or managed heap size.

**Check.** Run a cold start, busy scene, level transition, and return to the menu with device or process telemetry. Identify the owner of each peak, including where an asset exists in two representations.

### Reduce allocation churn in measured hot paths

Tag allocations with a subsystem and capture how often they occur and how long they live. Reuse buffers or pool uniform, frequently created objects where churn is material. Bound pool capacity, define what happens when it fills, and reset all state before reuse.

**Check.** Compare a burst-spawn workload with and without pooling. Measure frame-time tails, allocations per second, and retained bytes after the burst. Confirm event registrations and references do not survive a reuse unintentionally.

### Match reset boundaries to actual consumers

Use a frame or task arena for temporary data when its consumers finish together. Reset only after dependent work completes, including any graphics submissions that read its memory. Keep deferred completion data in storage whose owner survives the operation.

**Check.** Inject slow completion and cancel work during scene unload. In a development build, poison reclaimed scratch pages or use lifetime tooling and confirm late tasks never read freed storage.

### Distinguish fragmentation from accumulating ownership

Measure live bytes alongside the capacity or commitment of allocator regions. Where available, inspect the largest free block and allocation size classes. Compare successive cycle snapshots to find allocations that survive a return to baseline. Track intentionally retained caches and exercise their eviction policy.

**Check.** Alternate many small allocations with occasional large ones across repeated scene cycles. Observe whether a large allocation fails despite free bytes, or whether tagged live allocation grows with each repeat. Each result points to a different fix.
