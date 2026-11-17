# Player controllers

## Problem and design reasoning

**Problem.** An input produces inconsistent movement across terrain, or a jump fails even though the player pressed near an edge. Trace input, action eligibility, and the resulting collision or velocity separately.

**Design reasoning.** Quake's movement branches and Celeste's jump grace timer are documented in source. A design inference is to represent control context explicitly, then tune intent handling separately from movement constraints. See the shipped examples below for sources.

The player controller interprets intent, determines which actions are available, and passes movement and action requests to simulation. Define who owns position, velocity, and facing in each state before tuning.

## Shipped examples

**Quake (1996).** The published WinQuake source derives a desired movement vector from forward and sideways input. It uses friction and acceleration on the ground, a separate air acceleration path, and a separate branch for moving in water. A context can have its own response while sharing the same intent. [id Software, WinQuake player movement](https://github.com/id-Software/Quake/blob/master/WinQuake/sv_user.c)

**Celeste (2018).** The published player class handles locomotion through distinct states and applies horizontal and vertical movement with collision callbacks. It refreshes a short jump grace timer while grounded and uses it to accept jumps shortly after departure. This makes eligibility an explicit rule alongside collision. [Extremely OK Games, published player source](https://github.com/NoelFB/Celeste/blob/master/Source/Player/Player.cs)

The examples illustrate context-specific control in 3D and 2D. Additional traversal abilities can use the same intent-to-state structure while adding their own eligibility, transitions, and termination rules.

## Techniques and practical checks

### Sample commands once and define their lifetime

Map inputs into directional intent and held or pressed actions. Queue edge events until their simulation tick, consume each accepted edge once, and clear irrelevant events when control changes owner. Keep interaction requests distinct from continuous movement. For simultaneous commands, define priority or allow both when their effects commute.

**Check** with press and release between simulation updates, variable rendering rates, pause, respawn, and a control handoff. Log input time, accepted tick, consuming state, and any rejection reason. Confirm that a held action repeats only where the rules allow it.

### Set movement ownership per mode

Define ground, airborne, swimming, or other movement modes through the contacts and abilities actually used in the game. For each one, state which fields persist, which velocity components change on entry, which reference frame applies, and which component resolves collisions. Treat stun, ladders, knockback, and scripted traversal as explicit transfers of authority when they take over those fields.

**Check** by jumping at a doorway, landing during a dash, entering water on a slope, and interrupting scripted traversal. Trace state and velocity before and after each transition. Verify simultaneous conditions yield the documented result.

### Establish grounding and platform behavior

Use supporting contacts or a short shape query with a slope rule and a tuned separation tolerance. Track the supporting object's identity and frame-to-frame transform. Transfer platform motion while supported and specify how much support velocity a jump inherits. Keep this grounded record distinct from any jump grace timer.

**Check** on mesh seams, at the steepest legal slope, beneath a low ceiling, and on translational and rotating platforms. Walk off or jump from the same marked point at several speeds. Confirm visual and collision poses agree and that platform motion contributes only once.

### Tune the controller against measurable targets

Pick target acceleration time, stopping distance, air steering, and jump apex before tuning presentation. With constant downward acceleration and target height `h` reached at time `t`, use launch speed `2h/t` and gravity magnitude `2h/t²` as a starting point. Simulate the actual tick integrator to verify the result, including variable-height releases if applicable.

**Check** using marked lanes and gaps. Record speed, position, grounded state, and apex time for short taps, a full jump, reversals, and hitches. Repeat with each supported input device and after enabling animation.
