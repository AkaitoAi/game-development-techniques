# Game thread ownership

## Problem and design reasoning

Parallel systems can read half-published state or retain references past destruction even when their individual tasks finish on time. Give each piece of mutable state an owner and declare when another execution context can read a snapshot or submit a mutation. In a system with worker migration, define ownership by subsystem and phase while reserving specific threads for APIs that require thread affinity.

In *Destiny*, Bungie designed resource tracking to validate reads, writes, creation, and destruction against valid times in the frame. In *Killzone Shadow Fall*, Guerrilla moved entity updates into jobs and developed rules and runtime validation for their concurrent execution. A transferable inference is that identifying legal access is a prerequisite to increasing parallelism. [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-) · [Jorrit Rouwe, Killzone Shadow Fall: Threading the Entity Update on PS4, GDC Europe 2014](https://gdcvault.com/play/1020886/Killzone-Shadow-Fall-Threading-the)

## Techniques and practical checks

### Define ownership and affinity at the boundary

For a subsystem, record who mutates its state, who reads it, and when each operation is allowed. Distinguish a required platform or render thread from a logical owner that happens to run on a worker. Add assertions where code crosses those boundaries.

**Check** attempt a mutation from a worker during a read phase in a development build. Confirm the assertion identifies the subsystem, object, and allowed phase. Run the legal call through the intended owner.

### Publish a whole version

Build a snapshot or transfer buffer off the consumer's read path. Publish only after writes finish, using a synchronization mechanism consistent with the engine's memory model. Keep previous versions alive until all readers release them.

**Check** delay the last producer job and hold the previous frame's snapshot on a consumer. Verify that the consumer sees a complete version and that delayed work never overwrites its live buffer.

### Route cross-owner changes as requests

Have workers submit commands or results for the owner to commit at a defined phase. Copy the needed data or use generation-checked handles. Set an ordering rule where commands conflict, and bound how long results wait before becoming visible.

**Check** send competing changes while retiring the target and reusing its slot. Verify order, rejection of the stale command, and the measured visibility delay of valid results.

### Give teardown an explicit handoff

Stop accepting new work, let consumers finish or acknowledge cancellation, and release resources at an ownership boundary. Avoid waiting on another subsystem while holding a lock it needs for its completion path.

**Check** unload a level while reads, writes, and completions are in flight. Repeat with varied worker counts and deliberate delays. Verify completion, stale-result rejection, and absence of access after release.
