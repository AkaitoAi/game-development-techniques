# Physics world partitioning

## Problem and design reasoning

A large world cannot keep every collider active at full cost. Removing a distant cell too early can also invalidate a moving object's contacts or constraints. The released *Quake* server uses area nodes to organize linked entities and narrow collision searches. That is evidence for spatial organization inside one world, rather than a complete open-world streaming system. [id Software, Quake world collision code](https://github.com/id-Software/Quake/blob/master/WinQuake/world.c)

For *Horizon Forbidden West*, Guerrilla's Jolt transition specifically addressed streaming bottlenecks and interaction with multithreaded game-object updates. The GDC 2022 talk provides a modern shipped example of designing physics integration around data arriving during play. The partitioning and handoff protocol below is a portable recommendation derived from those concerns. [Jorrit Rouwe, Architecting Jolt Physics, GDC 2022](https://gdcvault.com/play/1027891/Architecting-AsterCore-Physics-for-Horizon)

## Techniques and practical checks

### Separate spatial cells from simulation ownership

Track which cell supplies static geometry and which simulation owner controls each dynamic body. Allow a body to overlap cell boundaries without creating a second authoritative copy. Publish new static collision at a controlled simulation boundary and retain references until active users finish.

**Check** move a large body across several cells while repeatedly reversing direction. Compare body IDs, collision shapes, and constraint references. Verify there is one authoritative body and continuous support at seams.

### Load ahead using motion and dependency bounds

Estimate required collision coverage from supported speed, load latency, body extent, and traversal links. Pin cells that support active constraints or critical interactions. Choose an explicit recovery or loading policy when traversal outruns prepared collision.

**Check** fastest traversal, teleportation, a long constrained assembly, and a deliberate slow load. Verify required collision exists before interaction and inspect the chosen fallback when it cannot arrive in time.

### Retire collision through a lifecycle

Mark a cell for retirement, prevent new claims, wait for dependent use to end or transfer, and remove its collision at the permitted boundary. Keep stable references for pending queries and validate their generation when results return. Track constraints that span cells.

**Check** unload a support cell while an actor stands on it, a query is pending, and a joint crosses its boundary. Verify each dependent either pins the cell or follows a documented recovery path.

### Measure partition cost and boundary correctness together

Instrument resident shapes, broad-phase candidates, insertion and removal work, and high-percentile physics-step time. Compare different partition sizes on the same route. Include dense cell boundaries because local peaks can outweigh average savings.

**Check** a crowded seam, repeated cell churn, and a cold traversal route. Compare contacts against a fully resident reference scene where feasible. Investigate missing hits, duplicate collision, insertion spikes, and unexpected memory retention.
