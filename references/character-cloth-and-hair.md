# Character cloth and hair

## Problem and design reasoning

Jackets, capes, and hair need to follow the body while retaining enough independent motion to show acceleration, contact, and wind. Skinning alone locks every vertex to an authored joint motion. Full simulation can introduce clipping, cost, and unpredictable poses. Decide which silhouettes and motions players actually see at the intended camera distance before choosing the representation.

*Tomb Raider* (2013) shipped real-time per-strand hair physics on PC through a collaboration between AMD and Crystal Dynamics. AMD describes the goal as making Lara Croft's appearance more lifelike. *Uncharted 4* (2016) included cloth, hair, and fur simulation in a wider art pipeline that Naughty Dog presented at GDC 2016. [AMD launch announcement](https://ir.amd.com/news-events/press-releases/detail/218/amd-and-crystal-dynamics-collaboration-thrusts-gamers-into-one-of-the-most-realistic-pc-gaming-experience-ever-with-the-launch-of-tomb-raider) · [Naughty Dog, Technical Art Culture of Uncharted 4](https://gdcvault.com/play/1023251/Technical-Art-Culture-of-Uncharted)

Design inference. A close camera can justify a richer silhouette and more simulated detail. Larger crowds and distant cameras can often keep the same recognizable shape with skinned meshes, bones, or a few simulated guides. Define these tiers by shot and platform cost.

## Implementation and practical checks

### Attach the simulation to animation

Pin roots and fitted garment panels to animated attachment points. Allow loose regions to move under constraints. For a decoupled render mesh, transfer motion from the simplified simulation surface or guides with stable bindings. **Check** walk, sprint, sharp turn, and deep crouch. View attachment points up close and measure how far the unpinned silhouette drifts from its authored limits.

### Keep contacts predictable

Use body proxies that follow animated joints. Prioritize contact regions that change the silhouette or visibly penetrate the garment. Test self-collision separately because it can cost more than collisions against a few body shapes. **Check** a cape against the shoulders during a spin and hair against a collar during a head turn. Record penetration duration and solver cost with and without the extra collision pairs.

### Define discontinuity behavior

Choose a simulation time step and bounded catch-up policy. Reseed the solver from a valid pose when a teleport, outfit swap, or loading transition invalidates previous velocities. **Check** abrupt position jumps, low frame rates, and pause-resume. Confirm that the mesh returns to a bounded state without stretching, carries no stale velocity after a reset, and leaves a smooth silhouette during normal frame pacing.

### Budget simulation and rendering independently

Measure constraints, guide count, collision cost, geometry submission, and transparency separately. Reduce solver detail at distance and choose a stable transition to cards, a skinned proxy, or a lower density mesh. **Check** one close-up and the maximum planned number of visible characters. Profile the worst frame, then track a character through each distance threshold to inspect silhouette popping, flicker, and motion discontinuity.
