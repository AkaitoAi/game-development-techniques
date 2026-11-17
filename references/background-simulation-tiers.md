# Background simulation tiers

## Problem and design reasoning

Large worlds can contain more active objects and systems than the frame budget permits. A simulation tier defines which rules continue at full fidelity, which can update less often, and which state is retained until activity resumes. Rendering distance and simulation relevance can be independent.

In *Minecraft: Java Edition* 1.18, Mojang shipped a simulation distance setting that limits where blocks, fluids, and entities are updated, independently of render distance. Its release notes explain that this lets players use a longer render distance with less CPU load. [Mojang's 1.18 release notes](https://www.minecraft.net/en-us/article/caves---cliffs--part-ii-out-today-java)

In *Assassin's Creed Unity*, Ubisoft discussed simpler AI for the large crowds populating Paris. A practical inference is to select the amount of simulation by which behavior players can currently observe or affect. For persistent world systems, define transitions and preserve the state that still matters when fidelity drops. [François Cournoyer at GDC 2015](https://www.gdcvault.com/play/1022411/massive-crowd-on-assassin-s)

## Practical checks

- **Check** define an update policy for full simulation, reduced update frequency, and dormant state. Pin any entity required by an active player interaction or quest, then move its region beyond the ordinary distance threshold and verify the interaction resolves.
- **Check** cross a tier boundary repeatedly with two players or cameras. Use separate thresholds or a dwell period where transition churn is costly. Count promotions, demotions, and frame spikes as the two observers move independently.
- **Check** suspend a region with in-progress work and reopen it both immediately and after a long absence. For each system, verify the intended policy for paused state, bounded catch-up, or an elapsed-time calculation. Check for duplicate rewards and missing scheduled events.
- **Check** teleport into a dense region and record frame time and the time until each required system becomes interactive. Bound optional promotion work over several updates while keeping collision and required interaction state ready before the player can use it.
