# World streaming

## Problem and design reasoning

**Problem.** A world looks loaded before collision or navigation is ready, or fast traversal exposes stale regions and frame spikes. Measure request-to-playable latency and readiness by subsystem.

**Design reasoning.** Insomniac documents retaining a region model while changing from scripted routes to a continuous city layout. An inferred principle is to base scheduling on actual traversal and to declare readiness across dependencies before exposing a region. See the shipped examples below for sources.

World streaming keeps only the content likely to be needed resident. It governs the journey from storage to playable state, including asset dependencies, object creation, collision, navigation and activation. For coverage, terrain authoring and quest topology, see [Large open worlds](large-open-worlds.md).

## Shipped examples

**Fuse (2013) and Sunset Overdrive (2014).** Insomniac's postmortem describes how Fuse used scripted trigger volumes to load and unload regions along mostly linear routes, with references keeping shared zones resident. Sunset Overdrive reused that region model but grouped content into hexagonal regions for continuous city traversal. The talk documents why loading collision and navigation coherently matters when another region can unload while AI is present. [Elan Ruskin, Insomniac streaming postmortem](https://media.gdcvault.com/gdc2015/presentations/Ruskin_Elan_SunsetOverdriveStreaming.pdf)

**Ratchet & Clank: Rift Apart PC (2023).** Nixxes added adaptive texture streaming driven by live measurements of available bandwidth. Its description identifies DirectStorage, NVMe and GPU decompression as contributors to responsive streaming on suitable systems. This example documents texture residency. Geometry, actor and navigation readiness need their own deadlines in a world streamer. [Nixxes project breakdown](https://www.nixxes.com/project/ratchet-clank-rift-apart/)

An authored trigger still fits a controlled route with known successors. Free traversal benefits from a demand policy that handles multiple possible routes and measures the hardware's actual delivery time.

## Techniques and practical checks

### Model residency as staged work

Track each package through requested, loaded and activated states. Separate background I/O and decompression from any thread-bound registration and GPU upload. Publish an area as traversable only after required collision and dependencies are ready. Register or enable navigation after that point, and remove cross-boundary navigation before releasing collision. Keep a coarse visual representation available where distant areas can be seen before activation.

**Check** by delaying each stage separately while approaching a cell at maximum speed. Trace when visuals, collision and navigation become ready. Confirm that actors use only legal paths and that failures lead to a supported fallback, such as holding at a transition or falling back to a verified coarse representation.

### Schedule against time to need

Predict a coverage envelope from movement speed, heading, reachable routes and camera visibility. Set minimum look-ahead from measured tail latency for the full request-to-activation chain, plus headroom. Rank critical collision and route assets ahead of decorative content. Update queued priorities when direction changes, and add hysteresis or a grace period before evicting cells still likely to be reused.

**Check** with sprinting, fast vehicles, reversals, diagonal boundary crossings and sudden changes of view. Run cold on the slowest supported storage. Log package deadlines, readiness misses, cancellations and p95/p99 frame time. Verify that reversing across a seam does not repeatedly reload the same contents.

### Bound in-flight work and handle abrupt jumps

Budget resident memory, decompression buffers, pending uploads and per-frame activation separately. Reference-count shared dependencies or use an equivalent ownership scheme. Give teleports or fast travel an explicit destination preparation step. Cancel obsolete requests with a generation token so a late completion cannot activate an abandoned destination. Use a defined fallback when the required working set exceeds its budget.

**Check** consecutive fast travels while I/O is delayed. Log peak working set, allocation failures, queue depth and frame spikes. Verify that only the final destination activates and memory returns to its expected steady state. Test small storage-cache budgets as well as ample memory.

### Preserve gameplay state through an unload

Use stable content IDs. Store durable changes, including collected or destroyed markers, separately from loaded objects. Reapply those changes before activating a reloaded cell. Give persistent actors crossing cell boundaries one active owner, and keep mission dependencies independent of scene object references.

**Check** by opening a door, collecting an item, moving an actor across a seam and unloading both sides. Return, then repeat across a save/load cycle. Verify the door and collection state, count active instances by ID and confirm that references to unloaded objects resolve according to the game's rules.
