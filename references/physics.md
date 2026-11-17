# Game physics

## Problem and design reasoning

**Problem.** Movement feels unreliable between ticks, objects penetrate thin barriers, or several physically plausible interactions combine into unstable results.

**Design reasoning.** Psyonix tuned the fixed-step solver around responsive car and ball interaction. Nintendo explored combined rules in a small prototype, suggesting that interactions central to the game deserve an isolated test space before scale or visual quality complicates diagnosis.

## How the techniques evolved

| Game | Documented approach | Transferable lesson |
| --- | --- | --- |
| Quake (1996) | The released WinQuake server source implements bounded swept movement and velocity clipping against collision planes in SV_FlyMove. Its stair handling evaluates paths around step obstacles. [id Software source](https://github.com/id-Software/Quake/blob/master/WinQuake/sv_phys.c) | Give controlled actors explicit movement rules and bound collision work per tick. |
| Rocket League (2015) | Psyonix used Bullet with discrete collision detection and an 8.33 ms fixed tick. The team modified the simulation and tuned the game around its responsiveness and car–ball interaction. [Jared Cone, GDC 2018 slides](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf) | Use a general solver as a foundation and tune the interactions central to play. |
| The Legend of Zelda: Breath of the Wild (2017) | Nintendo paired physics rules with material and element interactions and used a retro-looking prototype to explore combinations. [Nintendo developers, GDC 2017](https://www.gdcvault.com/play/1024562/Change-and-Constant-Breaking-ConventionsInteresting) | Prototype interactions in a small map before investing in production assets. |
| The Legend of Zelda: Tears of the Kingdom (2023) | Nintendo structured the expanded world around physics-driven gameplay and discussed the behavior of objects assembled into contraptions alongside the sound systems that react to them. [Nintendo developers, GDC 2024](https://gdcvault.com/play/1034667/Tunes-of-the-Kingdom-Evolving) | Treat composability as a content requirement and test assemblies across subsystems. |

These cases show an expanding scope for simulation, from actor locomotion through tightly tuned rigid-body interaction to construction systems in which many authored objects share rules. The implementations coexist as choices. A game can use a swept player controller alongside dynamic props and selective simulation of player-built machines.

## Implementation recipes

### Fixed simulation ticks and interpolated rendering

Accumulate elapsed wall time and advance the simulation by a constant interval while sufficient time remains. A candidate starting value is 1/60 s. Sample or queue input for named simulation ticks. Preserve the last two simulation transforms and interpolate for rendering using the remaining accumulator fraction. Capture and test the latency added by interpolation. Clamp long pauses and cap catch-up work according to the game's time policy. A networked simulation should define how clients resynchronize when they cannot keep up. [Glenn Fiedler, Fix Your Timestep](https://gafferongames.com/post/fix_your_timestep/)

**Verify** by replaying the same tick-indexed input at 30, 60 and 144 render frames per second, then with deliberate hitches. Compare state at equal tick indices. Record tick duration and catch-up steps and confirm that overload follows the intended policy.

### Swept movement and selective continuous collision

For a controlled actor, sweep its collision shape along the intended displacement, advance to the earliest hit with a small margin, and project the remaining motion onto the contact tangent. At a corner, preserve the accumulated constraints and stop when there is no legal remaining motion or the sweep budget expires. Handle initial overlap explicitly. Classify floors and steep slopes by contact normal. For fast rigid bodies, enable continuous collision detection for gameplay-critical collision pairs or use an explicit shape cast for a projectile whose expected behavior is a single hit. Check the engine's support for moving targets, rotation and triggers. [id Software, WinQuake movement](https://github.com/id-Software/Quake/blob/master/WinQuake/sv_phys.c) · [Erin Catto, Continuous Collision, GDC 2013](https://box2d.org/files/ErinCatto_ContinuousCollision_GDC2013.pdf) · [Box2D simulation documentation](https://box2d.org/documentation/md_simulation.html)

**Verify** with thin walls, acute corners, slopes, starting overlap, stairs, moving platforms and crossing targets. Include speeds at which travel per tick exceeds obstacle thickness. Log hit fraction, normal, approach speed and contact count. Require no tunneling or duplicate gameplay hits within the supported speed and geometry envelope.

### Composable rigid bodies and joints

Give physical pieces simpler collision geometry than their render meshes where feasible. Author mass, friction, restitution and joint anchors as gameplay data. Use constrained attachments for pieces that can articulate and consider a compound collider for a fully rigid assembly where piece-level dynamics are unnecessary. Apply propulsion as forces or impulses at world-space points to capture torque. Profile large assemblies and tune substeps or solver settings against measured errors. Feed impact effects from contact events with thresholds and per-pair limits to control repetition. [Nintendo, Tunes of the Kingdom](https://gdcvault.com/play/1034667/Tunes-of-the-Kingdom-Evolving) · [Box2D simulation documentation](https://box2d.org/documentation/md_simulation.html)

**Verify** with stacked props, long chains, extreme supported mass ratios, loops of joints and large player-built assemblies. Measure joint anchor error, maximum penetration, time to sleep and high-percentile step cost. Add scenario-specific tolerances, such as a maximum anchor error under a supported load or a maximum tick budget on minimum-spec hardware.

### Replays and determinism

Record initial state, tick-indexed inputs, random seeds and configuration. Compare canonical state hashes per tick with stable entity order and explicit serialization. Start with same-build repeatability, then repeat across any promised worker counts, builds or platforms. For rollback, validate that restored snapshots capture every value that affects future steps, including solver state or a documented mechanism to reconstruct it. If the required reproducibility cannot be verified, consider authoritative snapshots and reconciliation. Fixed intervals and solver determinism are separate requirements. [Erin Catto, Box2D determinism](https://box2d.org/posts/2024/08/determinism/)

**Verify** by locating the first divergent tick, dumping state for both runs and identifying the first divergent body, event or input. For rollback, restore from several checkpoints and compare against uninterrupted simulation at equal ticks. Include object creation and destruction and contact-rich scenes.
