# Character grounding and slopes

## Problem and design reasoning

A controller can visually touch a ramp while reporting airborne, accept a steep wall as ground, or snap back to a surface just after jumping. Grounding affects acceleration, jump eligibility, animation, and support tracking, so one ambiguous contact can produce several contradictory responses.

Shipped *Quake III Arena* separates having a ground plane from being in its walking state. The movement source rejects slopes below its walkable normal threshold, preserves a ground-plane constraint on steep slopes, and clears support when upward motion carries the actor away. These are explicit movement rules, rather than a direct use of any contact as grounded state. [id Software, player movement](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/bg_pmove.c)

Naughty Dog's *Uncharted* player-control talk discusses core movement together with animation layering and production costs. A transferable inference is that contact classification should belong to the movement system and provide consistent state to presentation. The specific grounding recipes below are engineering recommendations. [Travis McIntosh, Animation and Player Control in Uncharted, GDC](https://gdcvault.com/play/1012852/Animation-and-Player-Control-in)

## Techniques and practical checks

### Classify supporting contacts

Use the controller's up vector and a normalized contact normal. A candidate is walkable when their dot product meets the project's maximum-slope cosine and the body has usable support under its footprint. Handle zero-length normals and initial overlaps explicitly. Retain contact identity, point, normal, and classification for debugging.

**Check** ramps just below, at, and above the slope threshold, plus a vertical wall, an edge, and a seam between colliders. Verify that the contact classification matches the intended traversal rule and stays stable while standing still.

### Separate surface motion from gravity

Project desired ground movement onto the walkable surface and define whether uphill speed refers to travel along the surface or horizontal progress. Keep steep-slope sliding and airborne movement as explicit modes. Reconcile velocity against collision constraints before handing the pose to animation.

**Check** move uphill, downhill, and across a ramp at the same input magnitude. Measure travel speed and stopping distance. Verify a steep ramp cannot be climbed through repeated contact-state changes.

### Bound ground snapping

Use a short supporting query to follow small drops when the actor was recently supported and its motion permits attachment. Disable snapping during the intended upward jump phase. Keep snap distance separate from jump grace and step height so each parameter has a clear effect.

**Check** descend stairs, cross a shallow dip, walk off a ledge, and jump above a slope. Record snap displacement and state transitions. Confirm the character follows small surface changes while leaving the ground when the player jumps.

### Validate steps and recovery as complete moves

For stepping, test upward clearance, the forward sweep, and the final walkable landing. Compare that candidate with the ordinary collision result and select one under a documented rule. When geometry changes or the character starts overlapping, use a bounded recovery procedure and report failures with the scene and contact data.

**Check** obstacles below and above step height, stairs under a low ceiling, narrow supports, and a checkpoint spawning inside geometry. Confirm the controller chooses a legal final volume and cannot step through an overhang.

See [Moving platform programming](moving-platform-programming.md) for dynamic support frames and [Inverse kinematics](inverse-kinematics.md) for adapting the visible feet after gameplay grounding is resolved.
