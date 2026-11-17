# Gameplay serialization and snapshots

## Problem and design reasoning

Runtime snapshots capture the state needed to resume simulation, seek in a replay, or hand off between simulation instances. These uses have distinct latency and storage limits. In *Mortal Kombat XL* and *Injustice 2*, NetherRealm had to restore and resimulate several ticks within the frame budget. Blizzard's *Overwatch* replay talk describes building alongside the network model and delivering kill-cam reels before respawn. The practical inference is to define a snapshot around what its consumer must reconstruct and when it needs the result. [Michael Stallone, 8 Frames in 16ms, GDC 2018](https://www.gdcvault.com/play/1025471/8-Frames-in-16ms-Rollback) · [Philip Orwig, Replay Technology in Overwatch, GDC 2017](https://www.youtube.com/watch?v=W4oZq4tn57w)

## Techniques and practical checks

### Define the state required to continue a tick

Capture authoritative fields at a known simulation boundary. Include pending actions, timer values, and random generator state when future ticks depend on them. Rebuild derived caches from captured data where the rebuild cost fits the restore budget. Store object relationships as stable identifiers and resolve them after restoring entities.

**Check** snapshot a tick with an active timer, an entity queued for removal, and an upcoming random choice. Restore into a fresh instance, advance both instances with the same inputs, and compare gameplay outcomes and canonical state hashes.

### Bound history and recovery cost

Use a bounded ring of full snapshots when fast arbitrary restore fits the memory budget. For longer histories, consider keyframes and deltas. Give each delta a baseline identifier and limit the chain length so restore time has a predictable ceiling.

**Check** fill and wrap the history under peak entity counts. Attempt valid restores at the oldest retained tick and at each keyframe boundary. Verify that a missing baseline produces a controlled request for a full state or an explicit rejection.

### Publish snapshots after validation

Build a snapshot from one coherent tick using a barrier or immutable view. Decode into staging state, validate record counts and references, then publish the reconstructed state together. Include tick and format information so consumers can reject an incompatible or incomplete frame.

**Check** interleave capture with entity creation and deletion and restore a deliberately truncated local fixture. Verify that every successful restore sees a complete tick and that a failed restore leaves the active simulation unchanged.

### Budget capture and restore separately

Measure capture time, bytes per tick, rehydration time, and the simulation work that follows. Include presentation recovery in replay measurements and track confirmed side effects separately when resimulation can see an event twice.

**Check** restore the largest expected snapshot and resimulate the maximum supported window in a representative scene. Measure the worst-frame cost, check that gameplay converges, and verify that audiovisual effects or grants do not fire twice.
