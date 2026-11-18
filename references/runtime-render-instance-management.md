# Runtime render instance management

## Problem and design reasoning

A scene can contain many modular pieces whose render records must survive creation, movement, streaming, and destruction. Updating each draw directly from mutable gameplay objects makes parallel work and retirement difficult. Ubisoft's SIGGRAPH 2015 presentation describes a GPU-driven pipeline for *Assassin's Creed Unity*, motivated by complex scenes with modular buildings and characters. Bungie's *Destiny* talk discusses coherent rendering data and encapsulated synchronization. A transferable inference is to give render instances their own stable identity and publish controlled updates. [Ubisoft, GPU-Driven Rendering Pipelines, SIGGRAPH 2015](https://advances.realtimerendering.com/s2015/index.html) · [Bungie, Destiny renderer architecture, GDC 2015](https://advances.realtimerendering.com/destiny/gdc_2015/)

## Techniques and practical checks

- Give each render instance a handle whose lifetime distinguishes a removed object from a reused storage slot. Keep mesh and material references in a render-owned record. **Check** create, delete, and reuse many instances while delayed updates remain queued. Reject updates for the retired lifetime.
- Publish transform, bounds, and deformation changes as a coherent revision. Derive visibility bounds from the representation rendered that frame. **Check** fast motion, animated bounds expansion, and origin shifts. Verify culling does not use a new transform with old bounds.
- Update shared mesh and material bindings through explicit revisions. Invalidate cached draw or batching records when their dependencies change. **Check** changing an instance's material, mesh LOD, and visibility category during streaming. Compare with an equivalent instance created from scratch.
- Retire CPU records and GPU-visible indices only after outstanding work can no longer refer to them. Budget creation and removal bursts. **Check** unloading a dense region with several frames in flight. Track missing geometry, stale draws, retired references, and worst-frame integration cost.

For submission and culling architecture, see [GPU-driven rendering](gpu-driven-rendering.md). For buffers and descriptor retirement, see [GPU resource lifetimes](gpu-resource-lifetimes.md).
