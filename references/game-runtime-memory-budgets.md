# Game runtime memory budgets

## Problem and design reasoning

A build can fit in memory during ordinary play and fail during a workload overlap. Incoming resources, outgoing resources pending a fence, decompression scratch, and upload buffers can all remain live at once. Budgeting sets limits on those live costs and defines responses to pressure. Allocators determine how requests are served within those limits.

For *Titanfall 2*, Respawn's Chad Barb described permanent low-cost mip levels, a separate streaming budget with different settings for consoles and PC, and limits on in-flight operations. Maintaining a base representation and trimming detail gave the streamer a way to control residency without removing textures entirely. [Texture Streaming in Titanfall 2, GDC 2017](https://media.gdcvault.com/gdc2017/Presentations/Barb_Chad_EfficientTextureStreaming.pdf)

Naughty Dog's Matthew Trevelyan Johns described how texture costs and level limits constrained vehicle content in *The Last of Us Part II*. The team redesigned its vehicle pipeline to support more visual variety within those constraints. A design inference is that a documented subsystem budget can guide asset authoring as well as runtime eviction. [Driving Innovation: A New Vehicle Pipeline for The Last of Us: Part II, GDC 2021](https://www.gdcvault.com/play/1027001/-The-Last-of-Us)

## Implementation and verification

- Set budgets for each supported device class and memory domain. Count fixed residency, shared resources, reclaimable caches, staging, and reserved safety headroom. Distinguish requested bytes from committed and resident bytes. **Check** capture the same scenario with engine and platform tools. Explain material differences and verify shared resources count once against each applicable limit.
- Instrument high-water marks by owner and scenario. Record concurrent loads and delayed releases so a peak can be reconstructed. **Check** stress fast travel, repeated checkpoints, cold startup, and a rapid reversal across a streaming boundary. Compare steady-state and peak readings and locate the assets whose lifetimes overlap.
- Define soft pressure responses before hitting platform limits. Prioritize cancelling speculative work, lowering optional residency, and evicting disposable caches with bounded per-frame changes. **Check** lower the test budget under load and verify that pressure subsides without a retry loop, frame hitch beyond the agreed budget, or missing gameplay-critical resources.
- Turn limits into repeatable regression checks. Give each scenario a budget and track whether it changes when content or platform settings change. **Check** introduce a controlled staging spike and an intentional retained reference in a test build. Confirm the budget report catches both, attributes each to the correct owner, and recovers after removal.
