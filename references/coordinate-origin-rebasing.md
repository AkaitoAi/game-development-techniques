# Coordinate origin rebasing

## Problem and design reasoning

A floating-point position has less absolute resolution as its magnitude increases. At sufficient distance from the origin, small motion can become jitter or disappear between adjacent representable values. The relevant threshold is where errors exceed the project's contact, camera, or rendering tolerances. Streaming defines which objects are resident. Coordinate rebasing defines how resident objects remain numerically stable.

The *Kerbal Space Program* developers described the challenge of maintaining a stable physics simulation across solar-system distances and presented a floating-origin approach at Unite. In the shipped *Outer Wilds*, creative director Alex Beachum described keeping the player near the mathematical origin by moving the world in the opposite direction. The shared design motivation is local precision. A transferable inference is to keep a stable address for each object and derive local positions only where the subsystem needs them. [Kerbal Space Program developer talk, Unite 2013](https://www.youtube.com/watch?v=mXTxQko-JH0) · [Alex Beachum, The Making of Outer Wilds, around 31:53](https://www.youtube.com/watch?v=LbY0mBXKKT0&t=1913s)

## Practical techniques

### Separate stable addresses from local positions

Use a sufficiently precise absolute position or an integer sector plus a bounded offset for persistent addresses. Subtract the current anchor in the higher-precision representation before converting the result to a narrower local representation. Tag data with its coordinate space at subsystem boundaries.

**Check** place two objects a small fixed distance apart both near the global origin and at the maximum playable range. Convert both into the same local frame and verify their measured separation within the collision and rendering tolerances.

### Apply a shift at a defined synchronization point

Pick a boundary when physics, gameplay, and rendering can agree on a new anchor. For a pure translation by delta, add delta to the global anchor and subtract it from affected local positions. Update world-space caches and spatial query structures before readers resume. Preserve velocities under a pure coordinate translation.

**Check** force a shift while a character stands on a moving body and while a sweep is pending. Confirm no unintended impulse, missed contact, duplicated trigger event, or result interpreted in the wrong frame.

### Keep presentation history in a consistent frame

Transform previous and current positions into a common frame before interpolation, or invalidate history where a consistent transform is unavailable. Audit trails, particles, temporal rendering, world-space audio, and camera smoothing separately. Store velocities from consistent transforms so a shift does not appear as extreme motion.

**Check** capture the frame immediately before, during, and after a forced shift with a camera following a moving actor. Inspect motion vectors, trail continuity, and camera position. Repeat multiple shifts to catch error accumulation.

### Version local coordinates across asynchronous boundaries

Send stable coordinates or attach an origin identifier to local-space payloads. Convert incoming positions at the consumer using the relevant origin or rerun stale queries where their world state also changed. Save stable addresses so loading does not depend on the old session's anchor.

**Check** deliver an old-origin update after a shift and load a save with a different initial anchor. Verify both resolve to the same world landmark and that a second client using a different local anchor agrees.
