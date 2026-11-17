# Multiplayer movement and replication

## Problem and design reasoning

**Problem.** Local controls feel delayed, remote players move unevenly, or clients disagree about whether a fast interaction happened.

**Design reasoning.** Valve describes separating prediction, interpolation, and authority, and Psyonix reports different prediction quality for balls and cars. The transferable reasoning is to predict only where responsiveness wins enough to justify corrections.

## Shipped examples

**Counter-Strike: Source (2004).** Valve documents server-authoritative simulation, local client prediction, remote entity interpolation, and lag compensation for Source multiplayer. These mechanisms address different delays. The local client can react immediately to controls, remote motion can be rendered between snapshots, and the server can evaluate an action against an earlier view of the world when the rules call for it. [Valve, Source Multiplayer Networking](https://developer.valvesoftware.com/wiki/Source_Multiplayer_Networking)

**Rocket League (2015).** Psyonix presented a fixed-tick, server-authoritative simulation with client-side prediction of physics actors. When a significant correction arrives, the client rewinds to the corrected frame and advances the physics simulation to catch up. The team reported that the ball was easier to predict than player-driven cars, and that replay cost grows with latency. [Jared Cone, Psyonix, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)

## Techniques and practical checks

### Set authority and acknowledge input

Define which side owns final movement and action outcomes. In a server-authoritative model, send sequenced inputs and acknowledge the last processed input with the authoritative state. Specify how the simulation handles a missing input, a late one, and an ownership transfer.

**Check** with dropped, duplicated, and out-of-order traffic in a local network test. Verify that an action is applied once and that a seat change or respawn invalidates old input for the previous controlled object.

### Predict only the interactions that need it

Save local inputs by simulation tick. Apply predicted inputs immediately, then restore an authoritative state and replay unacknowledged inputs where needed. Expand prediction to nearby shared objects only when their immediate interaction matters and the replay cost fits the budget. Keep temporary presentation state distinct from authoritative events so replay can avoid duplicate effects.

**Check** with two players contacting the same moving object under low and high latency. Record correction size, replay duration, and time for the clients to converge. Repeat with different frame rates and server load.

### Interpolate remote movement with a bounded buffer

Order remote snapshots by server time and render from a short history. Define what happens when the buffer runs dry and how it handles teleports or discontinuities. Use consistent timing for objects whose relative motion players need to read together.

**Check** with constant delay, jitter, and bursts of packet loss. Observe stair-stepping, apparent delay, and time spent extrapolating. Test a teleport and a moving platform with a remote player riding it.
