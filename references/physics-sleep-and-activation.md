# Physics sleep and activation

## Problem and design reasoning

Resting props and ragdolls can consume simulation time or continually wake from small disturbances. Freezing them too aggressively can leave a suspended stack or delay a gameplay response. In *Diablo III*, Erin Catto describes rolling resistance as a way to settle rounded shapes sooner and shows how a ragdoll's shape and inertia affect settling. This connects authoring quality to simulation cost. [Catto, Diablo 3 Ragdolls, GDC 2012](https://box2d.org/files/ErinCatto_Ragdolls_GDC2012.pdf)

The released *QuakeWorld* source has a simpler resting path for tossed objects: it runs scheduled thinking and can skip movement when grounded. Modern connected rigid bodies require a broader sleep and wake policy. Box2D's developer explains why connected bodies need coordinated activation through their constraints. The recommended protocols below apply those principles to an engine integration. [id Software source](https://github.com/id-Software/Quake/blob/master/QW/server/sv_phys.c) · [Catto, Simulation Islands](https://box2d.org/posts/2023/10/simulation-islands/)

## Programming techniques and practical checks

### Specify activation ownership

Use the solver's supported activation operation when applying forces, changing attachments, or removing supporting geometry. Identify which gameplay operations require waking and which visual-only changes can leave physics asleep. Queue changes at a permitted simulation boundary.

**Check** a sleeping prop hit by a moving body, a scripted impulse, and removal of its supporting floor. Confirm movement begins at the intended tick and no stale callback mutates a replaced body.

### Settle connected bodies together

Preserve the physics engine's contact and joint connectivity when deciding whether an assembly can sleep. Tune shape distribution, joint error, and resting motion before raising sleep thresholds to hide instability.

**Check** a jointed chain, a stack, and a ragdoll on a slope. Track settling time and maximum drift, then disturb one end and confirm the connected assembly responds consistently.

### Keep sleep distinct from disabled simulation

Represent sleeping, explicitly disabled, and removed bodies as separate states. Specify what should happen to collision queries, sensor overlaps, and ongoing gameplay effects in each state using the target engine's documented behavior.

**Check** identical query and trigger fixtures for each state, including a body moved by script. Confirm that gameplay does not equate lack of movement events with destruction.

### Measure wake storms

Record awake-body count, island sizes, wake reasons, and step time. Treat large bursts as workload to budget when destruction or moving platforms disturb many objects.

**Check** a quiet scene followed by a mass disturbance, then recovery to rest. Compare worst-case step cost and time back within the frame budget on target hardware. Confirm no essential body remains frozen to meet the budget.
