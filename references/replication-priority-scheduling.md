# Replication priority scheduling

## Problem and design reasoning

Once replication has an eligible candidate set, a connection may have room for only a fraction of those updates. A scheduler that always favors the highest immediate score can starve background state. Bungie's shipped *Destiny* uses individually networked objects to prioritize traffic important to the local player, as described in Justin Truman's GDC presentation. The exact aging policy below is a recommended implementation, separate from that shipped architecture. [Bungie, Shared World Shooter: Destiny's Networked Mission Architecture, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Truman_Justin_Shared_World_Shooter.pdf)

Glenn Fiedler's technical article demonstrates accumulating object priority over time and clearing accumulated priority only for updates included in a packet. It provides an explicit way to let deferred objects move toward the front of the queue. Treat it as a technique example and validate its parameters against the target game's maximum tolerable state age. [Glenn Fiedler, State Synchronization](https://gafferongames.com/post/state_synchronization/)

## Techniques and practical checks

- **Represent urgency and age separately.** Derive current importance from gameplay role, recent change, interaction, and recipient needs. Add age or accumulated credit for eligible updates that have waited, with explicit latency targets for important classes. **Check** a dense interaction with continuously changing high-priority objects. Plot update age by class and verify less-important eligible objects still receive updates.
- **Charge the encoded cost.** Rank or bucket candidates, then estimate or measure the actual serialized size against the remaining connection budget. Retain deferred candidates for later consideration and define a path for an update that never fits a normal slot. **Check** a mix of small and large records. Verify large records eventually progress, the packet limit holds, and selection CPU time stays within budget.
- **Reset credit at the correct transition.** Specify whether credit is spent at selection, successful serialization, or transport acceptance, and what delivery guarantees remain elsewhere. Use entity generation and recipient identity in queued work so destruction or ownership changes invalidate stale candidates. **Check** force serialization failure, connection removal, entity ID reuse, and relevance changes. Confirm no update is silently starved or sent for the wrong lifetime.
- **Reprioritize without losing coherence.** Keep fields that require a shared revision together, and elevate newly relevant state enough to establish a valid baseline. Record the reason each candidate was included, deferred, or dropped. **Check** rapid camera turns, teleporting recipients, late joins, and an authority handoff. Verify baseline arrival precedes dependent deltas and visible state converges within its target.

Use [Network packet budgets](network-packet-budgets.md) to define available capacity and [Network replication layouts](network-replication-layouts.md) for encoding and baseline rules.
