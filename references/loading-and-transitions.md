# Loading and level transitions

## Problem and design reasoning

**Problem.** Loading appears finished while the destination lacks input, collisions, or a valid spawn, or the player loses their spatial reference after a level change.

**Design reasoning.** Valve's landmarks encode placement across a boundary, while Insomniac accounted for a destination that can be visible early. Both cases suggest treating the transition as a contract about destination readiness and continuity.

## Shipped examples

**Half-Life (1998).** Valve's SDK shows that the change-level save path builds a list of level connections. Each connection identifies a landmark used for placement in the destination. Give each boundary an explicit spatial reference and specify which state persists across it. [Valve's level change callback](https://github.com/ValveSoftware/halflife/blob/master/dlls/client.cpp) · [Valve's level connection data](https://github.com/ValveSoftware/halflife/blob/master/engine/eiface.h)

**Ratchet & Clank: Rift Apart (2021, PC port 2023).** Insomniac described how portal transitions and rapid loading constrained its lighting approach. Nixxes used DirectStorage and GPU decompression in the PC port to support rapid asset loading for dimension changes. When a destination is visible before the player enters it, budget its first view as part of the transition. [Brian Mullen, Insomniac, GDC 2022](https://media.gdcvault.com/GDC%2B2022/Speaker%2BSlides/RecalibratingOurLimits_Mullen_Brian.pdf) · [Nixxes PC port notes](https://www.nixxes.com/project/ratchet-clank-rift-apart/)

## Techniques and practical checks

### Define what crosses the boundary

List each value carried across, including player position, owned objects, held input, quest flags, and relevant timers. If spaces share a boundary, map the actor through a pair of anchors and define how view direction and velocity transfer. Choose a verified fallback spawn for invalid positions.

**Check** each direction through the boundary, including while sprinting, carrying an object, and holding movement. Reload immediately after arrival and compare location, facing, inventory, and quest state with the expected destination state.

### Gate control on gameplay readiness

Separate transition request, preparation, activation, and input release. Register destination collision, required scripts, and a usable first view before handing control to the player. Tie asynchronous callbacks to the transition that requested them and define an explicit recovery path for failure.

**Check** repeated requests and cancellation while loading, a stale callback arriving after a retry, delayed collision initialization, and a failed load. Confirm each request yields one legal arrival or recovery state and that input is restored.

### Choose presentation for the worst supported load

Pick a deliberate cut, loading screen, or in-world passage based on the experience and measured latency. Map any progress indicator to measured work or use an indeterminate state when completion time is unknown. For a visible destination, prioritize assets in the first view and bring remaining detail in after the transfer where appropriate.

**Check** cold and warm launches, the slowest supported device, and the largest destination. Record time from request to a controllable frame, the longest visible hitch, and the peak overlap in memory. Inspect camera, audio, and UI behavior throughout the transition.

### Protect checkpoint and transition commits

Define which state is durable if the process exits immediately before or after world activation. Use stable transition IDs to associate analytics and persistent rewards with their originating request. When activation fails, resume from a known safe checkpoint.

**Check** terminating the game at several points during the transition. Verify that reopening leads to a legal world state with no duplicate reward, missing checkpoint, or destination whose required content has not initialized.
