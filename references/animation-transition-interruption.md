# Animation transition interruption

## Problem and design reasoning

A transition can be interrupted while both its source and destination still contribute to the visible pose. Treating the destination as the only active owner can drop movement constraints, while retaining every interrupted owner can leave a hand, root, or interaction permanently claimed. Naughty Dog's *Uncharted: Drake's Fortune* presentation organizes animation around actions, layers, and controllers and describes the complexity created by expanding animation content. Clear transition contracts help control that complexity. [Christian Gyrling, Creating a Character in Uncharted: Drake's Fortune, GDC 2008](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf)

In Valve's shipped *Team Fortress 2* animation source, losing a controlling condition triggers explicit exit handling. The code also documents a case where returning early after equipment removal left the client in its viewing animation. These are concrete examples of interrupted state needing cleanup. The protocols below generalize that problem and are recommended implementation choices. [Valve, Team Fortress 2 animation state](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/shared/tf/tf_playeranimstate.cpp)

## Techniques and practical checks

### Specify interruption eligibility

For each transition, record which requests may interrupt, the earliest allowed phase, and the priority rule for several requests on the same tick. Distinguish an action's gameplay commitment from its visual blend. A cosmetic crossfade should not silently reopen or cancel a committed action. Make the chosen edge and rejected requests visible in a development trace.

**Check** request locomotion, damage reaction, and a new interaction at the same transition boundary. Replay the inputs and verify the same winner, action outcome, and released ownership every time.

### Blend from the actual current pose

Choose a strategy for interrupting a blend, such as capturing the current pose as a new source or retaining a bounded blend graph. Include root and additive contributions in the decision. Set limits on retained sources and define how cache entries are invalidated when the skeleton or active layers change.

**Check** interrupt a crossfade near its start, midpoint, and end, then interrupt the replacement. Inspect joint continuity, root velocity, source count, and evaluation time.

### Close the old action exactly once

Give each action an exit path for completion, cancellation, replacement, and owner destruction. Release interaction claims, movement overrides, and pending callbacks through that path. Presentation markers need an occurrence identity and a policy for whether the departing clip may still emit them while fading.

**Check** interrupt immediately before and after a marker, destroy the target during the blend, and reuse a pooled actor. Verify one terminal outcome and no callback applied to the replacement actor.

### Validate recovery with transition traces

Record active action instances, transition source and destination, weights, interruption cause, and claimed channels. Test the transition matrix in a compact scene using actual gameplay input and the final pose evaluation path. Include low frame rate, pause, slow motion, and a frame hitch.

**Check** interrupt every supported action from each permitted phase and verify a legal successor state. Compare the final pose and ownership against a fresh entry into that successor, while allowing the authored blend path to differ.

For base graph structure, see [Animation state machines](animation-state-machines.md). For routed markers, see [Animation event routing](animation-event-routing.md).
