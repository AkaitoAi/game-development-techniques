# Animation compression

## Problem and design reasoning

Long clips, many animated actors, and dense rigs put pressure on package size, resident memory, and pose sampling time. A codec can improve one budget while worsening another. Eidos-Montréal's Nicholas Fréchette describes a compact linear key-reduction baseline that was slow to decode and fell short of the accuracy required for cinematic clips. For another project, start by measuring error at visible contacts and measuring decode cost in a real animation workload. [Nicholas Fréchette, GDC 2017](https://nfrechette.github.io/public/simple_and_powerful_animation_compression_gdc2017.pdf)

## Shipped examples

- **Uncharted: Drake's Fortune (2007).** Naughty Dog's postmortem lists highly compressed animation among the measures used for crowds. A transferable inference is that a distant crowd loop can have a different fidelity budget from an animation that must hold a handheld prop in close-up. [Naughty Dog, GDC 2008 slides](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf)
- **Rise of the Tomb Raider (2015).** Fréchette identifies the game's engine as the context for his comparison and describes its existing linear key-reduction method. The talk examines a different approach using constant tracks and quantized samples. This establishes the shipped engine's baseline and the engineering motivation for the later approach. [Simple and Powerful Animation Compression, GDC 2017](https://gdcvault.com/play/1024009/Simple-and-Powerful-Animation)

## Techniques and practical checks

### Measure errors in the reconstructed pose

Decode the candidate using the same interpolation and transform composition as the game. Compare representative points on the reconstructed pose with the source. Local angular error matters more on a joint whose descendants sit far away. Set thresholds by actor role and by contact importance.

**Check** foot plants, hand grips, and face closeups at source frames and halfway between them. Record maximum end-effector displacement and inspect the worst frame at gameplay and cinematic camera distances.

### Remove constant data and tune precision separately

Detect constant tracks within an error threshold. Preserve the intended reference value and omit per-frame samples only where that value is adequate. Allocate precision separately to translation, rotation, and scale where the format permits.

**Check** a looping idle, a fast turn, a long root-motion clip, and a clip with animated scale. Compare byte counts and decoded poses with the original and verify that the loop seam remains smooth.

### Benchmark access patterns in a player build

Sample clips under the actual playback pattern. Contiguous storage can favor sequential sampling, while efficient random access helps scrubbing, rollback, and motion search. Include decoder setup and cache effects in the benchmark.

**Check** many actors sampling different clips, a synchronized crowd, and random seeks. Record memory and high-percentile animation CPU cost on the slowest supported device.

### Preserve gameplay contracts

Keep animation markers and authoritative root motion aligned with playback time. Include skeleton identity, codec version, and source settings in generated-asset invalidation. Preserve a source representation for recooking.

**Check** blending into an interrupted action and jumping across a marker during a hitch. Verify event counts, root displacement, and object contact after an import settings change.
