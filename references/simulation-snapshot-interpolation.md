# Simulation snapshot interpolation

## Problem and design reasoning

Snapshots arrive less frequently than render frames and can arrive unevenly. Rendering the latest received transform on every frame produces visible steps. Retaining older observations lets the renderer sample between two known states, at the cost of showing an older world. Treat that presentation delay as distinct from authoritative simulation time.

In shipped *Quake III Arena*, the client evaluates position trajectories at the two snapshot timestamps and interpolates their results. The source comments explain the preference for interpolation when newer player position data is available, even though a straight blend can approximate a curved path poorly. [id Software, Quake III Arena client source](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_ents.c)

Ubisoft’s Matt Delbosc described how *Watch Dogs 2* timestamps vehicle snapshots and samples at a time offset. The client interpolated between available observations or extrapolated when the target time was newer. The talk shows why choosing the offset matters for fast traffic and collisions, where too much delay separates the visible position from the position another peer observes. [Matt Delbosc, Replicating Chaos, GDC 2017, Snapshot Buffer and Time Offset slides](https://media.gdcvault.com/gdc2017/Presentations/Matt_Delbosc_Replicating_Chaos.pdf)

## Techniques and practical checks

- Store snapshots by simulation tick or source timestamp. Convert the local presentation clock to that timeline, subtract a delay, and find the two samples around the target time. For continuous positions, start with a linear blend weighted by elapsed time between those samples. **Check** snapshots at 0 ms and 50 ms arriving 80 ms apart. A presentation sample at 25 ms should land halfway between their positions regardless of arrival spacing. Include reordered and duplicate samples.
- Choose a different policy for missing successor samples. Hold the latest pose or extrapolate briefly according to the motion model. Cap the prediction horizon and measure how often the policy triggers. **Check** replay captured loss and jitter with steady motion and abrupt braking. Compare buffer underruns, correction distance, and presentation delay across candidate offsets.
- Give teleports, spawns, despawns, and authority handoffs an explicit boundary. Blend only compatible observations of the same entity and generation. Apply discrete state at its effective timestamp and sample linked objects on a consistent timeline where their interaction requires it. **Check** a moving object with an attached rider, followed by teleport and slot reuse. Confirm there is no sweep through intermediate positions and the old entity never reappears.
- Keep interpolated transforms in presentation storage. Draw authoritative samples, presentation samples, and the chosen target time together in a debugging overlay. **Check** compare simulation state under different render rates and interpolation delays with identical authoritative input. The state used to resolve interactions should remain identical while the visible paths can differ.
