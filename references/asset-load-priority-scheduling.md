# Asset load priority scheduling

## Problem and design reasoning

Speculative downloads, nearby geometry, and a player-selected item can compete for storage, decode workers, memory, and integration time. Priority on disk alone may still leave a critical asset waiting at a later stage. In Sunset Overdrive, high-resolution texture mips, long audio files, and unpredictable vanity combinations used loose loading through an asset manager that handled I/O and priority. Its streaming postmortem describes adapting to rapid open-world traversal. The scheduler below is a portable recommendation for meeting visible readiness deadlines across the whole pipeline. [Elan Ruskin, Streaming Tech in Sunset Overdrive, GDC 2015 slides](https://media.gdcvault.com/gdc2015/presentations/Ruskin_Elan_SunsetOverdriveStreaming.pdf) · [Insomniac, Streaming in Sunset Overdrive's Open World, GDC 2015](https://www.gdcvault.com/play/1022268/contactUs)

## Techniques and practical checks

### Prioritize the required result

Classify requests by player-visible need, estimated time to use, fallback availability, and work remaining. Give a critical parent request an appropriate dependency priority so a texture, skeleton, or collision asset cannot leave it blocked behind speculative work. Keep the policy inspectable in traces.

**Check** request a critical character whose dependency is queued as low-priority content. Verify the dependency receives the priority needed to meet the character's deadline and that its fallback policy is consistent.

### Apply budgets to each pipeline stage

Track reading, decompression, deserialization, resource creation, and world integration as separate queues. Limit their time, bytes, and concurrent memory according to the platform. Use measured queue age and actual completion latency to identify the stage preventing readiness.

**Check** replay the same request trace with slow storage and separately with a saturated integration thread. Verify diagnostics identify the different bottlenecks and that critical content becomes usable within the declared budget.

### Revise priorities without thrashing

Recompute urgency when the player changes route or selection. Use bounded preemption, preserve useful completed work, and age eligible requests so steady high-priority traffic cannot starve all lower-priority work. Deduplicate shared work and track the highest current consumer need.

**Check** reverse direction repeatedly while another request steadily ages. Measure wasted bytes, queue age, and fallback duration. Verify the shared dependency stays loaded while at least one consumer needs it.

### Define overload and shutdown behavior

When readiness cannot meet a deadline, select a documented fallback, loading gate, or reduced-detail representation. Give every consumer completion, cancellation, or failure. Retire cancelled work at a safe boundary and account for staged allocations until they are released.

**Check** exceed the expected request burst, then cancel most consumers and unload the area. Verify critical collision remains safe, cosmetic quality degrades as intended, and pending requests and staging memory return to baseline.

See [Asynchronous asset lifetimes](async-asset-lifetimes.md) for consumer leases and [World streaming](world-streaming.md) for choosing spatial content.
