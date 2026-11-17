# Runtime debugging tools

## Problem and design reasoning

A surprising gameplay result can span input, entity state, physics, and AI within a few ticks. A frame screenshot shows the final pose, while unstructured logs can make the initiating event hard to identify. Give the team a way to observe why a decision occurred while the game is running and review the same evidence afterward.

At GDC 2014, Guerrilla's Mika Vehkala presented ReView as used to debug multiplayer bots in *Killzone: Shadow Fall*. The session describes the shift toward explaining why an action was selected or rejected and shows ways to present relevant context for programmers and designers. [GDC 2014, Out of Sight, Out of Mind: Improving Visualization of AI Info](https://www.gdcvault.com/play/1020590/Out-of-Sight-Out-of)

The developers of *Ghost Recon 2* describe Gravy, a game-specific debugger that recorded timestamped messages, filtered them by data type and object, and offered playback and inspection of AI behavior stacks. Its original motivation was to understand existing AI behavior with limited documentation. The transferable inference is to capture domain decisions and stable object identities in a form the team can inspect across time. [Game Developer, Groovy Gravy: Tricking Out Your Custom Game Debugger](https://media.gdcvault.com/GD_Mag_Archives/Game.Developer.2004.09.pdf#page=20)

## Techniques and practical checks

### Record decisions at the point they happen

Emit a structured event with the tick, subsystem, entity identifier and generation, selected action, and relevant reason. For a gate, capture the predicate that failed and its observed value. Use a bounded in-memory history and reserve detailed recording for an enabled category or selected entity.

**Check** create a test where an NPC action is rejected by one known precondition. Find the failed gate and the relevant value from the trace alone. Destroy and respawn the NPC and verify the inspector distinguishes the entity lifetimes.

### Provide both spatial and temporal views

Display geometry or ranges in a lightweight spatial overlay, then link a selected object to a timeline. Include the coordinate space, capture tick, and authority for relevant values. Keep selection history so investigators can follow an object after it leaves view.

**Check** run two characters through a trigger while toggling the overlay. Select each event and compare the timeline, world geometry, and the actual tick on which the trigger fired.

### Capture a small, portable reproduction

Export a bounded time window with the build identifier, level, category filters, relevant inputs and configuration, and captured observations. Mark sampling gaps and missing dependencies so a recipient knows what evidence is available. Use recorded state for inspection where exact gameplay replay is unavailable.

**Check** send the capture from a staged intermittent issue to a teammate. Confirm the viewer opens the file, can filter to the affected entity, and clearly reports gaps and unsupported schema versions.

### Control overhead and access

Measure collection and display separately. Sample high-frequency values where appropriate and preserve the events necessary to reconstruct the ordering. Separate read-only diagnostics from state-changing controls, and expose tools only to the intended users and builds. *Factorio* documents separate access for some debug settings and makes timing and tick indicators available to players. [Wube Software runtime documentation](https://lua-api.factorio.com/latest/classes/LuaGameScript.html#allow_debug_settings)

**Check** run the same busy scene with capture disabled, capture enabled, and the overlay visible. Compare frame-time tails, allocation, dropped samples, and output size. Confirm a normal player build excludes developer controls and exports omit private player data.
