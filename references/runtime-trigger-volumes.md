# Runtime trigger volumes

## Problem and design reasoning

A trigger must know whether an actor is inside over time, even when the actor has several colliders, the volume is disabled, or a callback removes an entity. Valve's released Source SDK trigger code for the Half-Life 2 codebase maintains touching entity handles and distinguishes individual touches from the first and last qualifying occupant. Its disable-and-end-touch path explicitly accommodates callbacks that mutate the touch list. This makes trigger lifetime and cleanup part of the implementation contract. [Valve Source SDK, triggers.cpp](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/triggers.cpp)

Jason Gregory's GDC 2009 talk describes state-based scripting for the shipped Uncharted games. That is relevant context for turning spatial events into encounter states. The volume protocol below is a portable recommendation, rather than a claim about Naughty Dog's trigger implementation. [Naughty Dog, State-Based Scripting in Uncharted, GDC 2009](https://www.gdcvault.com/play/1730/State-Based-Scripting-in-UNCHARTED)

## Techniques and practical checks

### Separate overlap pairs from logical occupants

Track collision-pair identity and aggregate qualifying pairs by actor. Emit a logical enter when its count becomes positive and an exit when it reaches zero. Keep first-occupant and last-occupant events separate from per-actor events. Expose the filter that defines eligibility.

**Check** enter with a multi-collider character, remove one collider, and then leave. Add two actors together and verify exactly one logical enter per actor and one final-empty event.

### Define every terminal path

Specify how removal, disable, teleport, scene unload, and filter changes end membership. Use generation-aware actor handles and idempotent cleanup. Defer user callbacks or traverse a stable snapshot so a callback can remove another occupant safely.

**Check** disable an occupied trigger, destroy an occupant from its enter handler, and unload the volume while a character is inside. Confirm no retained effects, stale members, or duplicate exits.

### Reconcile continuous state with events

Use callbacks to update membership, then reconcile where the engine requires it after spawning, teleports, or saved-state restoration. Define whether reactivation synthesizes enter events or rebuilds state silently. Keep an encounter's one-time completion state separate from occupancy.

**Check** spawn inside, teleport across, and reload while overlapping. Compare the resulting encounter state with an ordinary walk-in, including a completed one-shot objective.

### Make timing and authority explicit

Queue physics callback data for a named gameplay phase when direct mutation would conflict with simulation. Record volume, actor, generation, tick, and transition. In networked play, choose the authority that commits rewards or encounter transitions.

**Check** repeat a boundary crossing at low and high render rates, with several physics steps in one frame. Verify the same logical event order and one authoritative grant.
