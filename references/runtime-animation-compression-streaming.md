# Runtime animation compression streaming

## Problem and design reasoning

Loading every compressed clip can consume too much memory, while an unexpected gameplay action may need a pose before a disk request can finish. Nicholas Fréchette's GDC presentation discusses compression work in the engine previously used by the shipped *Rise of the Tomb Raider*. His later ACL article explains a progressive approach that keeps essential samples resident and streams additional quality data. The article describes library technology, and does not establish that the shipped game used progressive streaming. [Fréchette, Simple and Powerful Animation Compression, GDC 2017](https://nfrechette.github.io/public/simple_and_powerful_animation_compression_gdc2017.pdf) · [Fréchette, Controlling animation quality through streaming](https://nfrechette.github.io/2021/01/17/progressive_animation_streaming/)

This guide concerns the sampling and residency contract after a codec has been selected. The protocols below are recommendations for keeping animation playable when compressed blocks arrive late, leave memory, or change version.

## Techniques and practical checks

### Define the minimum playable representation

Keep the metadata and baseline pose samples needed to evaluate an eligible gameplay action without waiting for IO. For predictable sequences, a segmented strategy can prefetch upcoming blocks. State the fallback for a missed segment explicitly and retain authoritative action timing independently of cosmetic quality.

**Check** disable optional quality streaming and run every immediately selectable action. Compare pose validity, root displacement, and action events against the full-residency run. In a segmented cinematic, delay the next segment and verify the documented fallback rather than an uninitialized pose.

### Make block boundaries sampleable

Define which block owns each time interval and how interpolation obtains samples on either side of a boundary. Include codec, skeleton, clip revision, and quality tier in residency identity. Treat loop seams and reverse or random sampling as explicit access cases.

**Check** evaluate just before, at, and just after every block boundary, plus the loop seam. Seek backward across an unloaded block and compare the resulting pose with a fully resident reference within the chosen error tolerance.

### Pin a coherent residency snapshot during evaluation

Acquire references to the compressed data and metadata before scheduling sampling jobs. Publish newly available blocks at a defined boundary. Retire old blocks after all readers finish, and reject completions for an obsolete clip revision.

**Check** pause a sampling worker, request eviction, replace the clip, and release the worker. Verify its data remains valid until completion and that a late old-version load cannot attach to the replacement.

### Budget quality changes and prefetch

Prioritize imminent actions and visible actors using the project's measured decode and transfer budgets. Track baseline and optional bytes separately. Measure visible error when quality changes, especially at planted feet and held props.

**Check** run a route with many actors choosing different actions, then constrain IO and memory. Record fallback frequency, animation CPU tails, bytes transferred, and contact error. Repeat a close camera cut and confirm quality recovery stays within the declared budget.

See [Animation compression](animation-compression.md) for codec error and decode-cost evaluation, and [Async asset lifetimes](async-asset-lifetimes.md) for request ownership.
