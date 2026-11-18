# Networked animation state

## Problem and design reasoning

Replicating an action's start without its identity and phase can leave late observers showing a different stage of the action. A correction can also restart a gesture that the local player already saw. Blizzard's shipped *Overwatch* uses Statescript for high-level weapons and abilities; Dan Reed describes synchronizing script behavior and its prediction as part of a reusable authoring system. This supports making action state inspectable across the network. The animation protocol below is a portable recommendation. [Dan Reed, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities)

Valve's released *Team Fortress 2* animation code selects gestures from gameplay conditions and animation events. Its stun handling explicitly exits the animation when the controlling condition disappears. This illustrates the need to reconcile presentation with current gameplay state. The source does not establish the proposed wire representation below. [Valve, Team Fortress 2 animation state](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/shared/tf/tf_playeranimstate.cpp)

## Techniques and practical checks

### Replicate the action contract

Give a network-visible action a stable type, per-actor instance identity, authoritative start tick, duration or rate where relevant, and cancellation or completion state. Resolve local clip selection from that contract and compatible content definitions. Keep cosmetic variation separate unless it changes another player's understanding of the action. A phase derived from the presentation timeline should use the same time basis as the displayed actor transform.

**Check** observe an action from its start and join visibility halfway through it. Verify that both clients show the appropriate phase, and that gameplay eligibility agrees even if cosmetic clips vary.

### Reconcile prediction by instance identity

Associate a predicted action with the input or request that started it. When confirmation arrives, match it to the existing action, adjust phase as needed, and define which corrections can blend and which require an immediate reset. Retain enough action history to recognize an already presented effect during replay. Record a cancellation reason when prediction is refused.

**Check** introduce latency, rejection, duplicate delivery, and reordered snapshots in an authorized test match. Confirm one logical action, one committed outcome, and a bounded visible correction.

### Define pose dependencies explicitly

Publish equipment, stance, movement mode, action identity, and attachment changes as a coherent presentation revision or specify how missing dependencies are deferred. An animation state that requires a new weapon should wait for a usable attachment or select a documented fallback. Keep collision and gameplay authority in the simulation while pose evaluation consumes the accepted state.

**Check** swap equipment during an action and remove an attachment before a delayed update arrives. Verify the observer never applies a previous action to the replacement item and recovers when the dependency becomes available.

### Trace observer timelines

Capture simulation tick, received action revision, presentation time, selected clip, phase correction, and emitted feedback identity. Compare local prediction and remote observation against the same authoritative action record. Include actor re-entry after relevance changes.

**Check** replay one trace at different rendering rates and with packet gaps. Locate the first phase mismatch and verify that recovery does not duplicate gameplay cues or leave a finished action looping.

For transport-side smoothing, see [Simulation snapshot interpolation](simulation-snapshot-interpolation.md). For feedback under replay, see [Resimulation-safe gameplay events](resimulation-safe-gameplay-events.md).
