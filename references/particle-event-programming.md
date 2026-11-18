# Particle event programming

## Problem and design reasoning

Particle collisions or deaths can request secondary effects and audio. A GPU simulation can produce those observations later than the gameplay frame that caused them. Reading results synchronously can stall the CPU, while unrestricted callbacks can create an event storm or target a reused emitter.

Bungie's *Destiny 2* particle architecture represents effects as graphs with parameter expressions that can execute on CPU or GPU. Brandon Whitley's presentation identifies iteration speed and performance as explicit production goals. The source establishes heterogeneous evaluation, rather than a particular callback protocol. The event-bridge rules below are recommendations for effects that cross those execution domains. [Brandon Whitley, The Destiny Particle Architecture, SIGGRAPH 2017](https://advances.realtimerendering.com/s2017/index.html)

Epic documents particle-data export to gameplay objects and notes that GPU exports carry additional latency and mapping work. Its audio documentation also distinguishes a one-shot sound that can continue after simulation stops from a persistent sound whose parameters and lifetime remain controllable. That is a concrete API example of why event delivery and spawned-effect ownership need separate contracts. [Epic, Niagara audio and particle-data export](https://dev.epicgames.com/documentation/unreal-engine/how-to-create-audio-effects-in-niagara-for-unreal-engine) · [Epic, export data interface](https://dev.epicgames.com/documentation/unreal-engine/API/Plugins/Niagara/UNiagaraDataInterfaceExport)

## Techniques and practical checks

### Define event identity and payload ownership

Describe spawn, collision, and death separately. Copy only the data the consumer needs, such as event kind, simulation step, emitter generation, particle identifier, position, and surface category. Treat particle-slot reuse as a new lifetime. Specify whether repeated contacts are individual events or one contact episode.

**Check** bounce one particle repeatedly, kill and reuse its slot, and deliver a duplicate batch. Compare received identities with expected occurrences and confirm downstream deduplication does not suppress a legitimate new particle.

### Use a bounded asynchronous bridge

For GPU events, allocate a bounded export buffer and collect completed batches without waiting for the current simulation frame. Associate each batch with its simulation revision and emitter lifetime. Specify overflow policy and record dropped events. Keep gameplay-critical hit detection on an authoritative path whose timing and completeness satisfy its rules.

**Check** delay several result batches, fill the export buffer, and remove the emitter before delivery. Verify the main thread does not block, dropped-event counters match the configured policy, and late data cannot invoke a replacement emitter's handler.

### Limit secondary emission and persistent effects

Give event-driven secondary effects their own budget, recursion limit, and lifetime owner. Apply cooldown or spatial aggregation where many nearby contacts would create repetitive audio. A one-shot effect and a tracked loop need different cleanup policies.

**Check** create a dense collision burst whose secondary particles can also collide. Confirm event depth, active effects, audio voices, and queued work stay bounded. Stop the source and verify tracked loops terminate while allowed one-shots complete.

### Make delayed observations visible in traces

Log simulation time, export completion time, dispatch time, effect identity, and dropped count. Draw the captured event point and the current source separately when debugging a moving emitter. Choose whether presentation uses the historic point, follows a retained target, or rejects observations beyond an age limit.

**Check** move and teleport an emitter while holding completed batches. Release them in a controlled order and verify the documented placement and expiry behavior. Repeat with pause and slow motion and compare simulation timestamps with the selected clock policy.

See [Particle system architecture](particle-system-architecture.md) for emitter storage and retirement, and [GPU readback pipelines](gpu-readback-pipelines.md) for staging-buffer ownership.
