# Data layout and cache locality

## Problem and design reasoning

An update can spend much of its frame budget waiting for memory even when it performs little arithmetic. Wube described this in shipped *Factorio* by separating bandwidth limits from latency limits and explaining why linked-list traversal made hardware prefetch difficult. The team added software prefetching for a subsequent entity while processing the current one. In a separate belt optimization, Wube used runs of connected belts and endpoint gaps to avoid visiting individual items during common updates. These examples address distinct sources of cost, namely waiting for a fetch and needing fewer fetches. [Wube on entity-update prefetching](https://www.factorio.com/blog/post/fff-204) · [Wube on belt data representation](https://www.factorio.com/blog/post/fff-176)

Chris Butcher's *Destiny* talk frames data formats, object lifetimes, and performance as assumptions that cross engine interfaces. An inference for redesign is to preserve a useful gameplay interface while choosing a backing representation from the dominant systems' access patterns. [Chris Butcher, Bungie, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Butcher_DestinyEngine_GDC2015_final.pdf)

## Techniques

- Describe each hot pass in terms of fields read and written, traversal order, and population. Record working-set size and frame-time distribution on target hardware. **Check** sweep entity counts through several working-set sizes, with clustered and shuffled iteration. Compare elapsed time and available cache or memory-stall counters under identical work.
- Keep data accessed together close together. Separate large cold payloads from hot records and consider arrays of fields for passes that use a narrow subset. Prefer arrays of small records when most fields are read together. **Check** compare alternate layouts using the same entities and mutation workload, including spawn and removal. Verify equal outputs and inspect both processing time and maintenance costs.
- Group work by stable behavior when it reduces indirection without changing rules. Keep persistent identity independent of dense slots that may be moved during compaction. **Check** interleave removals, spawns, and updates and verify handle resolution along with stable order where outcomes depend on it.
- Treat manual prefetch and alignment changes as target-specific experiments. Look first for removable work or easier traversal, then consider fetch distance and actual cache-line use. **Check** benchmark optimized builds on more than one device tier, including a small scene. Track regressions and keep the simpler layout when measured gains fall within run-to-run variance.
