# Game checksum programming

## Problem and design reasoning

Two simulation runs may appear identical until a small state difference changes a later decision. A checksum provides a compact comparison signal only when both sides hash the same logical state at the same simulation boundary. Raw memory hashing can instead compare addresses, padding, allocation order, or presentation caches.

Wube documents *Factorio* using deterministic lockstep and generating reports containing the server and client states after desynchronization. Michael Stallone's GDC 2018 talk on *Mortal Kombat X* and *Injustice 2* describes comparing many values at checkpoints within a tick. These shipped cases support an early detection strategy, while the canonical encoding and diagnostic hierarchy below are implementation recommendations. [Wube, Friday Facts 188](https://www.factorio.com/blog/post/fff-188) · [Michael Stallone, 8 Frames in 16ms, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Stallone_Michael_8FramesIn16ms.pdf)

## Techniques and practical checks

### Define a canonical checksum schema

Specify the fields that can affect future simulation, their encoding, stable record ordering, and byte order. Include counts and boundaries so different sequences cannot produce identical input bytes through ambiguous concatenation. Exclude pointers, uninitialized padding, and render-only state. Version the schema alongside build and content information.

**Check** construct equivalent states using different allocation and insertion orders and compare their encoded bytes and checksums. Change one gameplay-critical field, a record count, and a random-stream position in separate fixtures and confirm the detector observes each change.

### Sample matching ticks and phases

Hash a coherent view after an identified phase has completed. Record the simulation tick, phase, checksum schema, and state generation with the result. Complete relevant jobs or read a stable snapshot before traversal. Avoid sampling partially applied commands.

**Check** run a recorded input trace with varied worker timing. Verify equal-state comparisons use matching phase IDs. Pause one worker during a mutation and confirm the checksum capture waits or uses the intended prior snapshot rather than reading a mixed state.

### Partition diagnostics without hiding state

Keep a whole-state fingerprint and per-system or per-region fingerprints that can locate the first differing group. Define how entity insertion, removal, and movement between groups affect aggregation. For incremental checksums, retain a full recomputation as a diagnostic oracle so omitted mutation paths can be detected.

**Check** create, modify, migrate, and remove one entity while recomputing the full checksum at every test tick. Compare incremental and full results, then inject one known mismatch and confirm the report identifies the first measured system and phase.

### Retain evidence beyond the hash

Choose an algorithm appropriate to accidental divergence detection and account for collision risk. A matching finite checksum is evidence within that risk. When fingerprints differ, capture the input sequence and canonical state necessary for a detailed comparison. Keep histories bounded and record gaps when overload prevents capture.

**Check** introduce a known divergent input and reproduce the mismatch from the recorded build, seed, and tick. Verify the report includes both states needed for a field-level diff. Test history exhaustion and ensure it reports missing evidence instead of claiming a complete trace.

See [Game state diffing](game-state-diffing.md) for field-level reports and [Deterministic random streams](deterministic-random-streams.md) for reproducible random state.
