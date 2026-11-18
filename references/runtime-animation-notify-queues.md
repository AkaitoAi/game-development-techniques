# Runtime animation notify queues

## Problem and design reasoning

Animation evaluation and gameplay delivery can run at different cadences. A hitch may cross several authored markers, while throttled evaluation can accumulate a delivery burst. A queue needs a policy for time, capacity, and overdue work. Otherwise presentation can lag behind the action or one expensive catch-up frame can cause the next hitch.

Valve's released Source SDK, used for *Half-Life 2* and related games, tracks the previous event-check cycle, enumerates events crossed since that point, and calculates their occurrence times. It explicitly notes that its dispatch implementation does not handle negative frame rates. This supplies evidence for interval bookkeeping and a reason to define reverse-play behavior. It does not establish the bounded worker queue recommended below. [Valve, CBaseAnimating::DispatchAnimEvents](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/baseanimating.cpp)

Naughty Dog's developer talk describes synchronization challenges in the shipped *The Last of Us* melee system. The portable design inference is to preserve an occurrence's intended time when presentation delivery is delayed, and to keep the authoritative action's commitment under its declared owner. [Anthony Newman, Unsynced: The Last of Us Melee System, GDC 2014](https://gdcvault.com/play/1021172/Unsynced-The-Last-of-Us)

## Techniques and practical checks

### Define temporal occurrence identity

Collect marker crossings over a documented interval such as (previous, current]. Store playback instance, marker ID, loop index, intended tick or time, and payload revision. Specify explicit seek and reverse-play policies. Keep a cursor per playback instance so repeated queue drains do not re-enumerate the same interval.

**Check** evaluate an interval ending exactly on a marker, drain twice, then advance through a loop boundary and a seek. Compare the occurrence trace with the expected list and verify the selected seek policy.

### Bound storage and preserve required work

Estimate peak arrivals and put an explicit limit on queued storage. Separate required events from optional presentation work. Reserve space or backpressure the producer for required events. For a decorative burst, define a coalescing or drop policy that includes an observable count.

**Check** pause the consumer and produce more events than the configured capacity. Resume and verify that required events retain their order, memory stays bounded, and optional losses match the declared policy.

### Handle late events by meaning

Compare scheduled occurrence time with delivery time. A persistent loop can start at the corresponding phase, while an obsolete footstep can be skipped. Gameplay whose rule depends on exact simulation timing should run on the authoritative action timeline. Give tolerated lateness and catch-up limits explicit values.

**Check** delay delivery by a known number of ticks for a one-shot cue and a loop-start cue. Verify the selected skip or phase behavior and record the largest drain cost.

### Drain against a stable playback revision

Consume immutable batches at a defined phase. Validate actor and action generations before invoking a recipient. A callback can cancel or replace playback, so specify whether the rest of the batch remains eligible and reject entries belonging to the retired action.

**Check** let the first event replace the model and cancel the action. Drain the remaining batch and confirm that stale entries are rejected without reading retired model data.

For recipient selection, read [Animation event routing](animation-event-routing.md). For marker and gameplay commit semantics, read [Animation events and gameplay timing](animation-events-and-gameplay-timing.md).
