# Runtime sparse containers

## Problem and design reasoning

An update loop can waste time traversing empty slots after a high population shrinks. Compacting the storage can instead invalidate stored positions. Valve's released Source SDK, which includes game code for Half-Life 2 and Team Fortress 2, uses an index-based linked-list container with reusable element slots. Its entity registry separately maintains active and free lists. These are concrete examples of separating allocation capacity from live traversal. They support stable slot lookup, while linked traversal has its own locality cost. [Valve container implementation](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/public/tier1/utllinkedlist.h) · [Valve entity registry](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/shared/entitylist_base.cpp)

Timothy Ford's *Overwatch* Gameplay Architecture and Netcode talk at GDC 2017 describes using ECS to manage interacting gameplay features. The following sparse-to-dense design is a portable recommendation for a container with frequent lookup and iteration. The talk does not establish that Blizzard used this exact container. [Blizzard developer talk](https://www.gdcvault.com/play/1024001/-Overwatch-Gameplay-Architecture-and)

## Techniques and practical checks

- Choose an iteration and identity contract before choosing the representation. For a sparse-to-dense store, keep a sparse slot-to-position map and a dense position-to-slot map beside packed values. Define absent slots explicitly. **Check** randomized insertion and deletion against a reference map. Verify that every live slot resolves to the expected value and every dense position maps back to the original slot.
- If unordered removal swaps the last value into a removed position, repair the moved value's sparse mapping before publishing the change. Treat dense positions as temporary within a mutation-free scope. **Check** remove the first, middle, and last values and immediately resolve all remaining IDs. Repeat with one element and an empty store, then check callbacks that retained a position from before the removal.
- Make mutation during traversal an explicit operation. Queue structural changes until a barrier or document an iterator protocol that accounts for removals. Keep user callbacks from implicitly changing an actively traversed store. **Check** an update that deletes itself, deletes the next object, and spawns another object. Verify that the set of objects updated in the tick matches the chosen policy.
- Budget sparse capacity independently from live count. Page the sparse map or compact only through an explicit remapping operation if the identifier range becomes too large. **Check** high churn followed by a small population and widely separated IDs. Measure live traversal cost, peak capacity, and allocation count, then validate identity after any compaction.

See [Entity-component-system design](entity-component-system-design.md) for system-level ownership and [Game object generation counters](game-object-generation-counters.md) for rejecting references to reused slots.
