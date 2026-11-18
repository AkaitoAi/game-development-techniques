# Runtime UI layout invalidation

## Problem and design reasoning

An interface can spend CPU time recomputing unchanged geometry, or show stale geometry because a change did not invalidate the relevant cache. A change to text may also require parent measurement, placement, painting, and hit-test updates. *SimCity* (2013) shipped an HTML-based UI whose developer talk discusses dynamic layout, scaling, and efficient integration, showing why layout work matters in a complex game. The portable dirty-state protocol here is a recommendation rather than an attributed Maxis implementation. [Yee Cheng Chin, Maxis, GDC 2015](https://www.gdcvault.com/play/1022054/How-to-Implement-AAA-Game)

Epic's invalidation documentation distinguishes hierarchy, layout, and paint changes and describes caching unchanged UI work. Its architecture guidance also warns about feedback loops in which new layout depends on previous layout. The inference for another engine is to represent what changed explicitly and process affected dependencies in a stable order. [Epic, UI Invalidation](https://dev.epicgames.com/documentation/en-us/unreal-engine/invalidation-in-slate-and-umg-for-unreal-engine) · [Epic, Slate Architecture](https://dev.epicgames.com/documentation/unreal-engine/understanding-the-slate-ui-architecture-in-unreal-engine)

## Programming techniques and practical checks

### Distinguish the kinds of dirty state

Record whether a change affects structure, measurement, placement, paint, or hit testing. Propagate invalidation to parents when a child can change their desired size, and to descendants when the parent's constraints change. A color-only update can often reuse measured geometry. Confirm the target engine's dependency rules before narrowing invalidation.

**Check** change a color, a wrapped label, child order, visibility, and parent width separately. Trace which passes run and verify that both visual bounds and clickable bounds match the new state.

### Coalesce changes into a layout boundary

Apply a batch of model updates before measuring the affected roots. Mark each root once, process structural changes before measurement, then arrange before painting. If a callback changes layout during that process, schedule a controlled follow-up pass with diagnostics for repeated invalidation. Keep the dirty work set valid until its consumers finish.

**Check** update several counters and an inventory list in one frame, including an event raised during measurement. Count passes and confirm that the final visible state includes every update without an unbounded layout loop.

### Include external layout inputs in cache validity

Invalidate measurements when font availability, locale, text scale, display constraints, or content changes. A cached size is valid only for the inputs that produced it. Use explicit revisions where asynchronous text or image results can arrive after their original request has become obsolete.

**Check** switch language, resize the viewport, load a fallback font, and complete an image request after replacing its item. Verify wrapping, clipping, scroll extents, and alignment against a fresh layout.

### Measure cache benefit and correctness together

Expose counters for dirty roots, measured elements, painted elements, and time spent in each pass. Compare the same interaction with caching enabled and with forced recomputation as a diagnostic oracle. Retained render targets and animation policies have separate latency and memory costs.

**Check** capture an idle screen, an animated overlay, and a rapid update burst. Compare pixels and hit targets with the recomputed version, then inspect CPU time and memory to confirm the optimization helps its intended workload.

See [Gameplay data binding](gameplay-data-binding.md) for model updates and [UI list virtualization](ui-list-virtualization.md) for bounding the live widget population.
