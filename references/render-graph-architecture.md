# Render graph architecture

## Problem and design reasoning

A renderer with optional features can still draw the right image while carrying unnecessary passes, barriers, and allocations. Handwritten sequencing gives each pass little visibility into which later passes read its outputs. In his GDC 2017 talk, Yuriy O'Donnell described Frostbite's graph of render passes and resources as a way to let teams build modular rendering features on a shared engine while retaining efficiency. [FrameGraph: Extensible Rendering Architecture in Frostbite](https://www.gdcvault.com/play/1024612/FrameGraph-Extensible-Rendering-Architecture-in)

The shipped *Battlefield 1* and *Mass Effect: Andromeda* checkerboard case illustrates the underlying memory pressure. Graham Wihlidal's slides discuss explicit render target aliasing for the 4K transition and point to FrameGraph as the approach for future titles. The architectural inference is that a graph can derive and inspect resource lifetimes consistently where manual aliasing would otherwise require each feature author to reason about the entire frame. [4K Checkerboard in Battlefield 1 and Mass Effect: Andromeda, GDC 2017](https://media.gdcvault.com/gdc2017/Presentations/Wihlidal_4kCheckerboardIn.pdf)

## Implementation and verification

- **Make accesses explicit.** Declare each pass's reads, writes, resource usage, and affected subresources. Distinguish imported resources, such as the swapchain image, from graph-managed intermediates. A logical handle should identify the version the pass consumes when more than one pass writes a target.
  **Check** remove a depth-producing pass from a test graph and verify that compilation identifies its consumer and missing input. Change an optional feature and verify that the consumer reads the intended resource version.
- **Keep externally visible work rooted.** Start liveness from presentation, exported resources, readback, and declared side effects. Cull passes whose results are unused. Give history buffers explicit ownership across frames, and invalidate or replace them on resize, camera reset, or format change when the algorithm requires it.
  **Check** toggle an optional filter, request a screenshot, and resize the view. Confirm the unused filter is culled, the capture path still runs, and temporal history is reinitialized according to policy.
- **Allocate transient resources from scheduled lifetimes.** Establish first and last GPU uses after scheduling. Alias compatible allocations when lifetimes are disjoint and the backend supports the required synchronization and aliasing transitions. A fixed pass list alone cannot prove disjoint execution across queues.
  **Check** run with aliasing enabled and disabled on a representative scene. Compare output and peak transient memory, and validate a capture with graphics and compute overlap.
- **Inspect synchronization alongside performance.** Record resource states, barriers, queue waits, and submission order. Start with a correct serial schedule and enable parallel recording or async compute where captures show a measurable benefit.
  **Check** run alternating feature combinations over multiple in-flight frames. Validate cross-queue dependencies and compare frame time and GPU overlap against the serial path.
