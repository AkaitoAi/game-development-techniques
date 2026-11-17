# Mobile memory pressure

## Problem and design reasoning

A mobile game can exit during a scene transition or restart after an app switch even when its usual play footprint seems stable. Diagnose overlapping residency, OS reclamation, and app allocations as distinct contributors. At GDC 2012, Stéphane Khalil Jacoby described the iOS adaptation of *The Bard’s Tale* and its device-specific memory budgets. The team preserved fast loads by loading rarely used assets on demand and described releasing texture and audio caches on memory warnings. These are historical design decisions for that port. An inference for current projects is to make asset reconstruction an explicit part of the budget and choose reclamation behavior from current platform guidance. [The Bard’s Tale adaptation, GDC 2012 slides](https://media.gdcvault.com/gdc2012/slides/Missing%20Presentations/Jacoby_Stephane_Bringing_Large_Scale_Console.pdf) · [Android game memory overview](https://developer.android.com/games/optimize/memory-overview)

## Techniques and verification

### Budget device residency around transitions

Capture memory before, during, and after a high-water transition. Include the outgoing scene, incoming objects and textures, decode buffers, and queued graphics resources. Compare like-for-like OS measurements and use engine snapshots to attribute retained allocations. Choose an explicit margin for the weakest supported test devices. [Android memory monitoring](https://developer.android.com/games/optimize/memory-monitoring)

**Check** — Repeat a large level swap three times on a minimum-spec device. Timestamp unload, import, upload, and completion. Record both the peak and post-transition baseline. A transient peak calls for a different fix from stepwise growth that persists after returning.

### Bound concurrent decode and upload work

Choose which outgoing resources may coexist with incoming data. Cap concurrent decode buffers and release staging data as uploads complete. Test a loading screen or smaller interim presentation if old and new assets cannot safely overlap.

**Check** — Load a texture-heavy area from cold storage while sound is active. Confirm the peak stays within budget, then time the equivalent warm load and check for visual placeholders that remain after resources become ready.

### Give foreground and background separate budgets

Keep active gameplay resources, rebuildable caches, and persistent progress in distinct lifetimes. Use supported lifecycle signals to trim safe caches when backgrounding. Save at normal progress boundaries so an OS kill can recover even when no final callback runs. [Android low memory kills and lifecycle guidance](https://developer.android.com/games/optimize/vitals/lmk)

**Check** — Start a session, background it, then resume it both with the process still resident and after simulated process death. Verify state recovery and log time to usable input for both paths. Repeat the cycle to catch cache thrashing.

### Separate low-memory exits from ordinary crashes

Capture platform exit reasons where available and aggregate by build, device tier, and last gameplay phase. Pair production evidence with local memory captures. Keep performance work separate from the interpretation of exit records that lack a reliable cause. [Android exit classification](https://developer.android.com/games/optimize/vitals/lmk)

**Check** — On test hardware, compare recorded outcomes for a deliberate app crash, process termination, and a reproduced low-memory termination where the platform exposes that diagnosis. Verify the analytics and crash views classify them independently and preserve unknown cases.
