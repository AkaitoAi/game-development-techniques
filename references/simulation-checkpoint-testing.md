# Simulation checkpoint testing

## Problem and design reasoning

A regression trace can be long enough that replaying from its beginning slows every diagnosis. Starting from a serialized checkpoint shortens the loop, but a checkpoint that omits a pending event or random state can manufacture a different bug. Blizzard's Philip Orwig describes replay technology serving internal bug reproduction as well as player-facing features in shipped Overwatch. Factorio's desync reports preserve both client and server state for comparison. Neither source establishes the exact checkpoint test protocol below. It is a recommended way to validate restoration before relying on it for regression diagnosis. [Orwig, Replay Technology in Overwatch, GDC 2017](https://gdcvault.com/play/1024053/Replay-Technology-in-Overwatch-Kill) · [Factorio desynchronization reports](https://wiki.factorio.com/Desynchronization)

## Techniques and practical checks

### Define a coherent capture boundary

Capture after a named simulation phase, once earlier mutations have committed. Include state that affects future ticks, such as random stream positions, timer deadlines, entity generations, pending commands and configuration revision. Record the build and content manifest with the checkpoint. Separate derived caches that will be rebuilt from authoritative data.

**Check** capture with timers, queued spawns, status expiration and object destruction due on the next tick. Restore each capture and compare future authoritative outcomes against an uninterrupted run.

### Restore into a fresh world

Destroy the previous test world through its normal teardown path. Allocate objects, resolve stable references, reconstruct transient systems, and activate listeners only after the restored graph is coherent. Give external services substitutes appropriate to the test so restoring a run does not repeat a purchase or durable reward.

**Check** restore the same checkpoint twice in one process and once in a fresh process. Compare entity counts, subscriptions, pending work and canonical simulation state at the capture tick.

### Compare equal ticks with semantic state

Store tick-indexed input and externally determined results needed by the scenario. Hash a stable representation of authoritative state and dump a field-level difference at the first mismatch. Exclude allocator addresses and incidental cache layout. Preserve event identities when they influence future processing.

**Check** compare uninterrupted playback with checkpoint-plus-playback across several capture ticks, supported frame rates and worker configurations. Locate the first difference before interpreting later symptoms.

### Keep replay fixtures tied to compatibility

Version the capture format and declare which engine, content and configuration changes allow replay. Where migration is supported, validate it independently from the gameplay regression. Keep a short trace for each diagnosed bug and document the expected observable outcome.

**Check** run a valid fixture, an incompatible build, missing content, and a deliberately truncated checkpoint. Confirm that incompatibility is reported before gameplay begins and that the valid regression fixture fails when its targeted behavior is reintroduced.

See [Simulation record-and-replay testing](simulation-record-and-replay-testing.md) for trace capture and [Game state diffing](game-state-diffing.md) for first-divergence reports.
