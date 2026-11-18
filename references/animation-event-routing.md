# Animation event routing

## Problem and design reasoning

An authored marker can arrive at a character while the object that should handle it has changed. A weapon switch, cancelled action, or pooled actor can turn a valid marker into a stale callback. Valve's Source SDK weapon implementation explicitly routes operated-weapon animation events through the owning character and then into the weapon handler. That makes the character's relationship to the weapon part of delivery. [Valve, Source SDK weapon implementation](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/basecombatweapon.cpp)

Naughty Dog's shipped *The Last of Us* melee system is a developer case study in synchronizing animation with gameplay under interruption and multiplayer constraints. The portable routing protocols below are recommendations inferred from those ownership and synchronization problems. The talk establishes the gameplay problem rather than the exact envelope format proposed here. [Anthony Newman, Unsynced: The Last of Us Melee System, GDC 2014](https://gdcvault.com/play/1021172/Unsynced-The-Last-of-Us)

## Techniques and practical checks

### Route through the current action owner

Give a routed marker an actor handle, action instance ID, authored marker ID, and intended recipient role. Resolve a weapon or carried object through the action's captured relationship and verify that relationship is still valid. Specify whether an unequipped object may complete an already committed action.

**Check** switch equipment and cancel the action immediately before the marker. Reuse a pooled actor slot and deliver the old envelope. Verify that only the eligible action receives the marker.

### Separate marker identity from subscriber identity

Map authored marker meanings to typed payloads and recipient contracts. Subscribe presentation listeners with explicit lifetimes and keep authoritative changes under one declared owner. Report an unknown marker or missing required recipient with the clip, actor, and action instance.

**Check** remove an optional effect listener and a required gameplay recipient separately. Confirm optional presentation can be omitted, and a missing gameplay handler produces a diagnostic rather than a partially committed action.

### Queue delivery at a defined boundary

If animation evaluates on workers, collect immutable envelopes and deliver them at a gameplay synchronization point. Define ordering for markers from multiple layers and reject duplicates by action and marker occurrence where a blend emits the same logical cue twice.

**Check** vary worker completion order and blend two clips sharing a cue. Compare the delivered sequence and resulting state across runs, including a hitch that crosses multiple cues.

### Trace routing end to end

Record emission, recipient resolution, acceptance or rejection, and the resulting state change under the same action identity. Preserve reasons for stale-event rejection in development captures.

**Check** reproduce a cancelled interaction with a missing sound or delayed effect. Use one trace to identify whether the marker was absent, misrouted, rejected, or handled successfully.

For marker crossing and commit timing, see [Animation events and gameplay timing](animation-events-and-gameplay-timing.md).
