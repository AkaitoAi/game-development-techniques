# Procedural dungeons

## Problem and design reasoning

Dungeon generation has to produce playable routes across many seeds while preserving the tension that makes exploration worthwhile. A floor can be technically connected and still fail because a required key is behind its own lock, a one-way drop prevents recovery, or every optional room is a cheap detour. A useful design inference from the shipped approaches below is to establish the required route and its progression constraints at the floor level, then use authored rooms to control the quality of individual encounters.

**Spelunky.** Derek Yu and Andy Hull discussed the XBLA version at GDC 2011. The developer's account of generation describes establishing an entrance-to-exit path and selecting templates based on what connections each room needs. This gives designers control of the traversable shapes while leaving space for surprises inside them. [GDC 2011, The Full Spelunky on SPELUNKY XBLA](https://gdcvault.com/play/1014436/The-Full-Spelunky-on-SPELUNKY) · [Derek Yu, Spelunky](https://www.everand.com/book/882989338/Spelunky)

**Enter the Gungeon.** Dodge Roll's Dave Crooks described hand-authoring rooms with internal variation, assigning them types and eligible floors, then joining them at runtime using rules specific to each floor. The documented goal of the floor rules was to reproduce the qualities the team considered good dungeon design across generated runs. [Crooks interview on dungeon generation](https://mcvuk.com/development-news/random-mechanics-and-the-art-of-being-predictably-unpredictable/)

## Techniques and practical checks

### Validate progression as state

Represent rooms, directed exits, and gates explicitly. Track inventory or progression flags in the reachability search when they affect which exits can be used. Keep the graph for seed diagnostics.

**Check** a key before its lock, a key behind its lock, mutually locked branches, and a one-way drop. Search from the real starting loadout and confirm that the exit and required encounters are reachable.

### Give templates connector contracts

Author room templates with connector positions, directions, clearances, and encounter tags. Match rotated or mirrored templates only when their transformed sockets and movement constraints remain valid.

**Check** every allowed orientation and socket pair. Visit the seam using the actual gameplay collider and movement rules. Confirm that doors open, pathfinding crosses the seam, and nearby encounters have enough space to begin.

### Design pacing across the floor

Set bounds for required-route length, intervals between recovery opportunities, and what optional branches can trade for a reward. Choose placement after the route has been assigned so the dressing and encounters respect traversal constraints.

**Check** a representative seed batch. Record route length, longest stretch without recovery, reward distance from the main path, and repeated room frequency. Play the seeds that produce the extremes.

### Make generation failures reproducible

Save the seed, generator version, content revision, and rejection reason. Set a bounded retry policy and define a safe layout for exhausted attempts. Keep failed seeds as fixtures while the affected generator revision is supported.

**Check** a sparse template pool, blocked optional branch, and impossible connector request. Confirm the result is deterministic for retained fixtures and the fallback respects the same exit and progression guarantees.
