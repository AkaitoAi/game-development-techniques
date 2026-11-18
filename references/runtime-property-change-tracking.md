# Runtime property change tracking

## Problem and design reasoning

Repeatedly polling every field wastes work, while missing one mutation can leave replication or another consumer stale. Valve's released Source SDK, used for games including *Half-Life 2* and *Team Fortress 2*, wraps network variables with setters that mark changes and exposes explicit mutable access that also marks a change. This demonstrates putting tracking at the mutation boundary. It is a network dirty-state mechanism, not evidence that arbitrary observers can safely execute inside those setters. [Valve, Source SDK network variables](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/public/networkvar.h)

Bungie's *Destiny* job architecture validates data access phases. A portable inference is to give change publication an owner and phase, especially when consumers can mutate state or run on different threads. The observer delivery rules below are recommendations. [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-Destiny)

## Techniques and practical checks

### Define what counts as a change

Route writes through a field owner or a clearly defined mutable-access scope. Specify equality by value type and consumer semantics. A UI may use a display threshold while authoritative state and replication require the actual value change. Keep the original value when a consumer needs a before/after pair.

**Check** assign the same value, a tiny floating-point delta, a changed element inside a collection, and a whole replacement object. Verify that each relevant consumer receives the expected revision and that display thresholds do not suppress authoritative updates.

### Publish coherent changes at a boundary

Accumulate dirty properties while an operation updates related state, then publish a committed revision. Choose whether consumers receive every intermediate event or one coalesced current state. For state observers, expose a coherent snapshot rather than references into data that can change during consumption.

**Check** update maximum health and current health in one operation while a UI observer checks their relationship. Assert it sees the committed pair. Update one property repeatedly and verify the documented notification count.

### Make observer mutation rules explicit

Specify when additions and removals take effect during delivery. Queue reentrant writes for a subsequent publication phase or define a bounded reentrant policy. Pair subscriptions with owner lifetimes and validate identity when delayed deliveries target recycled objects.

**Check** let one observer remove itself, remove a later observer, add a new observer, and write another property during notification. Compare delivery order with the contract. Destroy and reuse the target before releasing a delayed notification.

### Validate tracking against a reference comparison

In a diagnostic fixture, compare full before/after state with the dirty set after supported commands. Track the owning object, field, revision, and mutation site when a field changes. Use sampled or debug-only comparisons when a full scan is too costly for normal play.

**Check** run command sequences containing collection edits, load restoration, and pooled-object reuse. Fail the fixture when changed persistent or replicated fields are absent from tracking. Measure notification work during a burst and verify that coalescing preserves the final state.

See [Gameplay data binding](gameplay-data-binding.md) for UI consumption and [Runtime event subscription lifetimes](runtime-event-subscription-lifetimes.md) for listener teardown.
