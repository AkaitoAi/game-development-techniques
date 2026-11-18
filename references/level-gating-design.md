# Level gating and route dependencies

## Problem and design reasoning

A route can be geometrically reachable while its encounter, story state, or return path is unready. Gates control those dependencies. Their design problem is to make a restriction understandable and reversible when appropriate, rather than accidentally trapping a player. This guide concerns spatial access, not experience levels or skill-tree progression.

In David Shaver's GDC 2018 talk, *Invisible Intuition*, a long drop in *Super Metroid* illustrates a valve that prevents immediate return. Treat this as Shaver's analysis of the shipped level, not a statement of Nintendo's internal reasoning. The useful distinction is between blocking forward movement and committing the player behind them. [Shaver's slides](https://media.gdcvault.com/gdc2018/presentations/DShaver_Invisible_Intuition_GDC2018.pdf)

The authored *Level Design Book* distinguishes hard gates, bypassable soft gates, one-way entrances, and shortcuts. It recommends exposing a lock before its key and reminding players where the newly acquired solution applies. The portable reasoning is that remembering a recognizable obstacle creates a navigation goal. [Gate design guidance](https://book.leveldesignbook.com/process/layout/typology/gates)

## Techniques and practical checks

### Model access explicitly

Record each gate's prerequisite, opening event, persistence, direction, and fallback. Express combinations such as all switches versus any valid route explicitly. Give permanent and encounter-local state different ownership. Keep decorative doors separate from actionable locks.

**Check** Enumerate prerequisite combinations and enter from every adjacent region. An unavailable route must show the intended reason, and every valid combination must open it.

### Teach the unmet condition

Use a consistent shape, interaction response, sound, or readable text to communicate the missing capability. Place the solution so its relationship to the earlier obstacle can be understood. The recommendation is to test recognition before adding extra navigation markers.

**Check** Ask unfamiliar players what blocks them and where they would return after obtaining the solution. Record wrong explanations and repeated dead-end visits.

### Make commitments deliberate

Before a one-way drop, check supplies, checkpoint placement, companion access, required tools, and the next safe destination. Decide which sequence breaks are supported. A soft gate needs a functional continuation when a skilled player bypasses its intended encounter.

**Check** Cross with minimum resources, leave enemies alive, and bypass the intended trigger. Each supported route must retain an achievable objective and recovery path.

### Persist world truth

Opening a shortcut should update collision, presentation, navigation, and saved state together. Apply a restored state without replaying rewards or story events. Validate both sides of a gate after streaming or loading.

**Check** Save before activation, during opening, and after passage. Reload from both sides and confirm consistent access, no repeated rewards, and no permanent softlock.
