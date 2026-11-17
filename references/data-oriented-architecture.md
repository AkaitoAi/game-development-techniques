# Data-oriented game architecture

## Problem and design reasoning

An update that touches thousands of entities can spend its budget following pointers, loading fields it never uses, or repeatedly doing identical work. Wube's *Factorio* optimization notes describe grouping consecutive belt sections into transport lines with contiguous item data and moving whole stretches by adjusting a shared offset when conditions allow. Bungie's *Destiny* GDC talk describes a job-based frame with system-level pipelining, data parallelism, and runtime verification of access against frame phases. A useful inference is to design the representation around measured operations, then introduce parallelism where ownership and dependencies are clear. [Wube, Optimizations for Factorio 0.14](https://www.factorio.com/blog/post/fff-148) · [Bungie, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Techniques and practical checks

### Lay out data for the operation

Record which fields a frequent loop reads or writes. Compare compact records with separate arrays for the fields the loop actually visits. Keep cold authoring metadata elsewhere when it measurably bloats the working set. Preserve a stable lookup from game identity to storage index.

**Check** compare the same update across small and representative large populations. Capture frame time, memory footprint, and available cache metrics. Verify equivalent results under insertion, removal, and reuse.

### Represent shared changes once

Look for contiguous runs, groups, or regions that often share the same transformation. Store shared changes once and materialize individual values only at boundaries that require them. Define when the group splits or merges and who owns exceptional cases.

**Check** compare the optimized path with a simple per-item reference on uninterrupted motion, a blocked boundary, and edits inside the group. Verify output and ordering on each tick.

### Batch homogeneous work and defer structural edits

Group work by the fields and operation it uses. Choose a batch size by measuring scheduling overhead and load balance. Stage structural changes until the end of an iteration phase so readers see a consistent layout.

**Check** run with one worker and with the normal worker configuration. Stress uneven batches and simultaneous create and remove requests. Verify results, measure scheduling overhead, and check how long workers wait at the phase boundary.

### Keep data layout behind a domain interface

Expose operations in game terms and keep raw array indices internal. Validate writes at the boundary, and make debug tooling translate physical storage back into game identities. Convert one hotspot at a time so correctness and cost changes are attributable.

**Check** replace a storage implementation while replaying the same fixture through its normal callers. Verify snapshots match and that an old handle cannot refer to a newly inserted element.

See [Memory management](memory-management.md) for allocation and [Multithreading and job systems](multithreading-and-jobs.md) for scheduling and synchronization.
