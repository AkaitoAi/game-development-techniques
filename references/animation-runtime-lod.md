# Animation runtime level of detail

## Problem and design reasoning

Evaluating every animation graph, constraint, and cosmetic bone every frame wastes work on actors whose motion is barely visible. Animation LOD chooses what work to perform and how often to perform it. Clip compression reduces the storage and bandwidth of the motion data used by those evaluations.

Ubisoft's François Cournoyer described how *Assassin's Creed Unity* pooled high-detail NPC representations and used simpler behavior to animate the larger crowd. The stated goal was to scale the number of persistent, interactive NPCs while hiding transitions between representations. [Massive Crowd on Assassin's Creed Unity: AI Recycling, GDC 2015](https://www.gdcvault.com/play/1022411/massive-crowd-on-assassin-s)

The *Resident Evil: Operation Raccoon City* rigging talk describes driving multiple detailed rendering rigs from one animation skeleton. A design inference is that a stable set of shared motion inputs gives runtime tiers a better chance of agreeing about pose and phase when their presentation work differs. [Rigging a Resident Evil, GDC](https://www.gdcvault.com/play/1015533/Rigging-a-Resident-Evil-Inside)

## Implementation and verification

- Score significance using projected size, player focus, recent visibility, and gameplay importance. Calibrate thresholds with hysteresis and a minimum dwell time. **Check** repeated threshold crossings, zoom changes, and a camera cut. Measure transition rate, frame spikes, and exposed pose pops.
- Keep the simulation clock and gameplay-critical motion data current while sampling distant presentation poses less often. Specify how root motion, gameplay events, collision sockets, and attachments are owned before throttling. **Check** replay the same sequence at full and reduced presentation tiers. Compare authoritative transforms and event counts, including events that cross an interval with no pose evaluation.
- Strip tiers by cost and dependency. Trim cosmetic constraint passes or auxiliary bones only when every remaining consumer has a valid substitute or an invariant transform. **Check** fixtures for props, cloth anchors, attachments, and character swaps. Force every tier and compare results against the full evaluation.
- Budget both steady work and promotion bursts. Spread noncritical promotions over frames, reserve capacity for visible or interactive actors, and seed their new poses from current state. **Check** a crowd reveal, teleport, and maximum actor count on minimum hardware. Track pose jobs, tail frame times, promotion backlog, and the time before critical actors reach full presentation.
