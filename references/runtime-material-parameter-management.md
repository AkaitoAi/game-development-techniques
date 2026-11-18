# Runtime material parameter management

## Problem and design reasoning

Several systems may try to change the same material value. A damage flash, customization color, and wetness update can overwrite one another. Creating a material object for every update also risks uncontrolled instance growth. A runtime contract needs to define which values are shared, which belong to one renderable, and how changes reach rendering.

Natalie Burke's GDC 2016 talk explains vertex animations shipped in *Destiny*, including shader functions driven by engine parameters and vertex colors. It documents a way to add motion detail efficiently alongside skeletal animation. Natalya Tatarchuk's GDC 2015 talk describes *Destiny*'s job-based renderer and its implications for feature design. Treating material updates as owned input data with a controlled handoff is a portable implementation inference, rather than a claim about Bungie's exact parameter API. [Animating With Math, GDC 2016](https://www.gdcvault.com/play/1022965/Animating-With) · [Destiny's Multithreaded Rendering Architecture, GDC 2015](https://www.gdcvault.com/play/1021926/Destiny-sMultithreaded-Rendering)

## Techniques and practical checks

### Define parameter scope and ownership

Classify each parameter as global, shared by a material family, or specific to one renderable. Define its type, unit, range, default, and owner. Keep authored material definitions separate from per-instance runtime overrides.

**Check** two actors sharing an authored material. Apply a local hit flash to one and a global environmental change to both. Verify the local effect stays isolated and pooled actors regain their defaults on reuse.

### Compose competing contributions

When multiple systems affect one appearance, collect their contributions and resolve them through a documented rule. For example, combine tint layers in a fixed order or take the strongest eligible flash. Keep gameplay timers and shader time inputs consistent with the chosen clock.

**Check** overlap damage, highlighting, and a status effect. Vary activation and expiry order and compare the final parameter values with the composition rule, including pause and respawn.

### Publish values through a stable render boundary

Let gameplay update owned parameter state, then transfer a coherent snapshot to rendering. Resolve names to validated handles outside hot update loops. Where supported, update instance buffers or property blocks without cloning the authored material.

**Check** rapidly toggle values while spawning and removing renderables. Trace revision and object identity through the handoff. Verify that a late update cannot modify a recycled target and that a frame sees a coherent parameter set.

### Track instance and update costs

Record live material instance count, parameter upload bytes, dirty objects, and draw batching behavior. Update changed values and group compatible transfers. Release transient material instances through their actual owner.

**Check** repeatedly enter and exit an effect-heavy encounter. Compare equivalent memory snapshots, instance counts, render-thread time, and batching. Confirm that counts stabilize and visual overrides remain correct at each supported quality tier.
