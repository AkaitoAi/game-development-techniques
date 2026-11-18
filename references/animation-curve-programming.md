# Animation curve programming

## Problem and design reasoning

Authored curves can control continuous values such as a hand-contact weight, shader deformation, camera offset, or sound parameter. The same curve can produce different results when its input clock, units, interpolation, and blend rules differ between consumers. Discrete event markers and continuous samples need different contracts.

Naughty Dog's *Uncharted 2* character pipeline presentation includes incoming animation-control curves and a face-pose node, documenting authored values as part of a production pipeline. It does not specify the portable runtime curve representation below. [Judd Simantov, Uncharted 2 Character Pipeline, GDC 2010](https://media.gdcvault.com/gdc10/slides/simantov_judd_uncharted2_character_pipeline.pdf)

Natalie Burke's talk examines mathematical vertex animation in shipped *Destiny*, including engine parameters and vertex-color controls. Its documented objective includes efficient motion that complements skeletal animation. A transferable inference is to make each continuous control's sampling domain and contribution explicit before combining it with other animation. [Natalie Burke, Animating With Math, GDC 2016](https://gdcvault.com/play/1022965/Animating-With)

## Techniques and practical checks

### Give each curve a sampling contract

Record its input domain, such as seconds or normalized clip phase, and its output meaning and units. Declare interpolation, endpoint behavior, loop wrapping, and the policy for an absent channel. Validate finite keys and legal input ordering when importing.

**Check** sample known fixtures at keys, between keys, outside endpoints, and on both sides of a loop seam. Compare results with the authoring preview and reject malformed or ambiguous input.

### Combine contributions deliberately

Assign an owner for each property and define how curve contributions combine. A contact weight might use a prioritized layer, while a cosmetic displacement might add several bounded terms. Apply identity defaults to missing channels so switching clips cannot retain an old value accidentally.

**Check** blend clips where one contains a channel and the other omits it. Add a second writer, interrupt the blend, and verify the output follows the declared rule and returns to its default when playback ends.

### Preserve shape when optimizing

Choose curve compression or lookup-table resolution using output error measured in its consumer's units. Include extrema and sharp changes when comparing a cooked representation with the source. If gameplay uses a threshold, evaluate whether compression can move the crossing to another simulation tick.

**Check** sample the source and optimized representation densely and at authored extrema. Record maximum error, threshold-crossing ticks, memory, and evaluation cost on representative content.

### Publish samples with their pose revision

Evaluate curves against the same playback state that produced the consuming pose where synchronization matters. Cache using clip revision, playback phase, and any blend inputs that affect the result. Preserve required curve updates when visual animation is throttled, or choose a supported approximation and validate it.

**Check** compare full-rate and reduced-rate animation while recording curve values, pose revision, and consumer output. Reload a curve asset and verify stale cached samples cannot overwrite the new revision.

See [Animation layer composition](animation-layer-composition.md) for joint ownership and [Runtime audio parameter control](runtime-audio-parameter-control.md) for audio consumers.
