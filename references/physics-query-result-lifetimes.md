# Physics query result lifetimes

## Problem and design reasoning

A raycast can return a correct hit whose buffer, collider, or gameplay recipient is gone by the time a later task consumes it. The architecture question is who owns the result and what world revision it describes. NVIDIA documents that PhysX scene hits contain actor and shape pointers, that requested flags determine which fields are valid, and that callbacks can receive successive chunks of touches. These contracts make retaining an arbitrary hit pointer a separate lifetime decision. [NVIDIA, PhysX scene queries](https://nvidia-omniverse.github.io/PhysX/physx/5.4.1/docs/SceneQueries.html)

For the shipped *Horizon Forbidden West*, Jorrit Rouwé describes designing Jolt around streaming and multithreaded game-object updates. That talk establishes the concurrency problem and the engine's goals. The result-envelope and generation checks below are portable recommendations derived from that problem, rather than a claim about Guerrilla's exact gameplay API. [Jorrit Rouwé, Architecting Jolt Physics for Horizon Forbidden West, GDC 2022](https://gdcvault.com/play/1027891/Canadian-Gaming-Technology-Conferences)

## Techniques and practical checks

### Copy only valid fields into owned storage

Create a result envelope containing request identity, sample tick, validity flags, hit fraction, and the fields the consumer requires. Retain raw engine references only under an explicit documented lifetime contract. Copy callback data before its scratch storage is reused, and report truncation separately from an empty result.

**Check** query a fixture with three colliders, copy its result, then run a second query using the same scratch buffer. Assert the first envelope retains its hit count and positions. Repeat with a result buffer smaller than the hit set and verify overflow is visible.

### Resolve target identity when applying the result

Map physics bodies to gameplay IDs with a generation or equivalent reuse discriminator. At consumption, resolve the recipient and target again and check that both still belong to the request's scope. A body pointer or storage index alone cannot establish that a replacement object is the intended target.

**Check** hold a hit in a test queue, destroy its target, reuse the body's gameplay slot, and release the queue. Assert the replacement receives no hit action and the rejected completion records its reason.

### Define an age and revision policy

Use current-state queries for decisions that require an immediately valid contact. For tolerant consumers such as ambient occlusion estimates, accept a bounded result age and revalidate movement or scene revision. Sampled local coordinates require the corresponding transform or an explicit conversion before later use.

**Check** sample a moving platform, advance it several ticks, then consume the result. Assert a current-clearance consumer rejects or requeries while a configured tolerant consumer follows its stated age and displacement limits.

### Retire requests before releasing their resources

Give every queued query one terminal outcome. Cancel consumption when its owner exits, and keep query buffers alive until the physics backend has finished writing. Publish completion to the appropriate gameplay phase after that boundary.

**Check** unload the requesting scene with several queries pending, then deliver completions in reverse order. Assert no callback mutates the unloaded scene, every request terminates once, and buffer counts return to baseline.

See [Physics query architecture](physics-query-architecture.md) for choosing and scheduling queries, and [Game object generation counters](game-object-generation-counters.md) for reuse-safe identity.
