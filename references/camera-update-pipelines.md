# Camera update pipelines

## Problem and design reasoning

A follow camera can jitter even when its individual movement formulas are correct. Sampling a character before its final movement, applying smoothing twice, or letting an effect overwrite collision correction changes the displayed result. In the released *Quake III Arena* code, the client processes snapshots, predicts player state, calculates the view, and then adds view-dependent presentation. This provides a concrete example of camera timing as part of a frame pipeline. [id Software, client view source](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_view.c)

Travis McIntosh's GDC 2012 talk on *Uncharted 3* documents managed follow, aim, melee, cover, spline, and animated cameras, with priority and fade controls exposed to designers. It describes camera shake as a separate additive layer. The portable recommendation is to make those stages and their state ownership explicit so each camera can consume a coherent target pose. The stage protocol below is an implementation proposal. [Naughty Dog, The Cameras of Uncharted 3](https://gdcvault.com/play/1015514/The-Cameras-of-Uncharted)

## Practical techniques

### Sample a coherent target pose

Record whether each camera follows a simulation pose, an interpolated presentation pose, or an animation attachment. Run it after the chosen source finishes updating. Carry the sample timestamp with diagnostic output so a late camera update can be traced to its input.

**Check** walk and rotate on a moving platform at low and high rendering rates. Log target and camera timestamps and capture relative position. Look for alternating old and new samples.

### Give each stage a contract

Resolve the active mode, compute desired framing, apply mode smoothing, solve collision clearance, and apply permitted presentation effects in a defined order. If an effect moves the camera enough to cross geometry, include that motion in the final clearance policy.

**Check** orbit beside a doorway with shake enabled, then disable each stage in turn. Verify which stage first produces an invalid final pose.

### Reset history on discontinuities

Handle teleports, respawn, target replacement, and origin shifts as explicit events. Decide which velocity and smoothing history to retain and initialize the new state from a valid pose.

**Check** teleport while orbiting and replace the target during a transition. Confirm the camera reaches the intended location without moving through the intervening world.

### Publish one final view

Give rendering, view-space feedback, and any camera-relative UI a common finalized view or a documented alternative. Cache the base result when several render passes need the same frame, and advance camera timers once.

**Check** enable a second render view or capture pass. Verify camera shake and blend time advance once, and that projected markers align with the rendered subject.

For framing policy, read [Camera and controls](camera-and-controls.md). For mode handoffs, read [Runtime camera blending](runtime-camera-blending.md).
