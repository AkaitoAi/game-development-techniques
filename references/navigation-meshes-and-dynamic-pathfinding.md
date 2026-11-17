# Navigation meshes and dynamic pathfinding

## Problem and design reasoning

A walkable floor does not guarantee a usable route. Agents can have different clearance, traversal abilities, and access to links that change at runtime. Kojima Productions documented combining long-range navigation over fragmented terrain with support for player-placed crossings for *Death Stranding*. A transferable inference is to separate durable connectivity from mutable links so one bridge edit need not force a rebuild of the whole world. [Death Stranding: An AI Postmortem, GDC AI Summit](https://gdcvault.com/play/1027144/AI-Summit-Death-Stranding-An)

## Shipped examples

- *Left 4 Dead* uses a navigation mesh for movement and derives flow distance from the survivor start to mesh areas. That second use gives encounter systems a measure of progress along the playable space. [Mike Booth, Replayable Cooperative Game Design, GDC 2009](https://cdn.akamai.steamstatic.com/apps/valve/2009/GDC2009_ReplayableCooperativeGameDesign_Left4Dead.pdf)
- *Death Stranding* combines a hybrid approach to long routes with navigation over dynamic bridges and ladders in rugged terrain. [Death Stranding: An AI Postmortem, GDC AI Summit](https://gdcvault.com/play/1027144/AI-Summit-Death-Stranding-An)

## Techniques

- Bake walkability from collision geometry with explicit clearance, climb and slope requirements per supported agent size. **Check** a doorway and ramp immediately above and below each threshold, then compare projected endpoints and actual physical traversal.
- Represent jumps, ladders, and changing crossings as directed links with eligibility and a traversal action. **Check** both directions, bridge removal during approach, and bridge removal while traversing. Verify that abort or completion follows the intended rule.
- Invalidate paths by affected region or link revision and stagger replans under a per-frame budget. **Check** many agents approaching the same closing door. Measure queue depth, replan latency, and time without a valid path.
- Distinguish unreachable, partial, and complete routes in debug output and gameplay contracts. **Check** targets on disconnected islands and overlapping vertical floors. Verify that partial routes never report arrival and that search costs are visible.
