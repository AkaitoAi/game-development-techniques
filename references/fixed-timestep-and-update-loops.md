# Fixed timesteps and update loops

## Problem and design reasoning

Rendering, input arrival, and simulation can all advance at different rates. If gameplay consumes input or advances cooldowns once per rendered frame, a slow frame can change what happens in the game. If the scheduler tries to repay an unlimited simulation backlog, the extra work can prolong the hitch.

In the released *Doom* source, `TryRunTics` checks elapsed and available command ticks before running game ticks. It also gives the menu a chance to update when waiting for commands takes too long. This is evidence of explicit scheduling between command availability, gameplay advancement, and UI responsiveness. [id Software, `d_net.c`](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/d_net.c)

Psyonix's Jared Cone describes the shipped *Rocket League* physics schedule at 120 Hz. Smaller steps reduced penetration and made contact outcomes more consistent, while increasing computation and the cost of network corrections. The transferable inference is to choose the rate using the interaction and machine budgets, then make timing behavior explicit at the scheduler boundary. [GDC 2018, It IS Rocket Science! The Physics of Rocket League](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)

## Techniques and practical checks

### Separate clocks and define update phases

Read elapsed time from a monotonic clock and accumulate eligible simulation time. Consume fixed-sized steps while enough time is available. Give each simulation step a tick number and define when input, gameplay, physics, and deferred mutations happen. Drive presentation timers that must work while paused from a separate clock.

**Check** run the same preassigned tick inputs at 30, 60, and 144 rendered frames per second. Confirm that the simulation advances the expected tick count and that actions resolve in the same phase and order.

### Retain input edges until a tick consumes them

Store a button's held state and queue edge events with an ordering policy. Consume each edge once in its assigned tick. Decide how to handle events sampled during pause, focus loss, or a delayed network input interval.

**Check** tap a button entirely between simulation ticks, then render a frame that executes three ticks. Verify the tap is observed exactly once. Test press and release close together, focus loss with a held button, and resume.

### Publish simulation snapshots for presentation

Render from previous and current committed simulation snapshots. In a typical accumulator loop, interpolate with the fractional remainder divided by the step duration after normal catch-up. Define the interpolation rule for simulation debt and reset snapshot history for teleports, respawns, and world loads.

**Check** render a steady camera follow at an uneven frame cadence. Verify smooth motion, measure the added display delay, and confirm that a teleport cannot draw an intermediate position.

### Set a catch-up and suspend policy

Give each host frame a work budget and measure pending ticks when the cap is reached. Choose whether to slow simulation, recover from an authoritative snapshot, or defer optional work according to the game's rules. Handle resume as a separate lifecycle transition so suspended time enters the simulation only when intended.

**Check** inject a one-frame stall and a sustained overload, then suspend and resume. Inspect backlog, per-frame tick count, UI responsiveness, and what happens to cooldowns and timers.
