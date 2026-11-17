# Network interest management

## Problem and design reasoning

Sending and evaluating every game object for every connected player makes server work grow with both world population and player count. Players still need prompt updates for nearby interactions, owned objects, and shared objectives. Epic documents using persistent, shared data to make replication lists less expensive in Fortnite Battle Royale. Bungie says networking Destiny's objects separately let it prioritize the traffic most important to the local player. An implementation implication is to give candidate selection and update priority independent budgets. [Epic, Replication Graph](https://dev.epicgames.com/documentation/en-us/unreal-engine/replication-graph?application_version=4.27) · [Justin Truman, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Truman_Justin_Shared_World_Shooter.pdf)

## Shipped examples

- **Destiny (2014).** Bungie's *Shared World Shooter: Destiny's Networked Mission Architecture* describes separate object updates competing for capacity within a shared space. That separation lets the game prioritize objects relevant to what the player is doing. [Bungie, GDC 2015 talk slides](https://media.gdcvault.com/gdc2015/presentations/Truman_Justin_Shared_World_Shooter.pdf)
- **Fortnite Battle Royale (2017).** Epic illustrates the Replication Graph with Fortnite's approximately 50,000 replicated actors and up to 100 players at match start. The graph maintains data across frames and shares work across connections, including grouping objects by location or behavior. [Epic, Replication Graph](https://dev.epicgames.com/documentation/en-us/unreal-engine/replication-graph?application_version=4.27) · [Epic networking team demonstration](https://forums.unrealengine.com/t/unreal-engine-livestream-networking-in-4-20-the-replication-graph-july-19-live-from-epic-hq/113725)

## Techniques and practical checks

### Build candidate sets once where possible

Maintain a spatial index and explicit memberships for owned objects, party data, and match objectives. Update index entries when entities move cells or change membership. Gather nearby and applicable shared collections, then deduplicate by entity identity.

**Check** a sparse world and a crowded match. Compare candidate construction CPU time, deduplicated count, and bytes sent per connection while verifying objective and owned-object updates.

### Apply relevance and priority separately

Determine eligibility from authoritative game rules, then prioritize eligible updates by importance, state change, and elapsed time. Give essential interactions reserved capacity and establish a maximum acceptable update age for other active state.

**Check** under a repeatable simulated low-bandwidth profile. Verify excluded state stays absent, interactions meet the project's latency target, and lower-priority active state makes progress.

### Specify entry and exit behavior

When an entity enters a client's interest set, send a baseline before dependent changes. On exit, invalidate its presentation and track enough identity to distinguish a later re-entry from a replacement entity. Consider distance hysteresis or dwell time for cosmetic updates near a boundary.

**Check** movement along a cell boundary, teleportation, and returning to a previously visited region. Verify the client shows each entity once, restores its current state on re-entry, and avoids rapid churn.

### Treat viewpoint and gameplay ownership as different inputs

A camera cut may change nearby objects without changing who owns an inventory or participates in a mission. Define separate relevance inputs so a temporary view change preserves the data required for gameplay. Keep the server responsible for visibility rules.

**Check** a spectator transition, remote-controlled view, and reconnect after changing regions. Inspect expected subscriptions and initial state, then confirm old view state is released.
