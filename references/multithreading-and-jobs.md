# Multithreading and job systems

## Problem and design reasoning

**Problem.** Idle cores coexist with a busy main thread, or workers still miss frame deadlines because work waits for results or resources.

**Design reasoning.** Bungie's job graph makes dependencies explicit, and Massive prioritizes short in-frame work separately from tasks that can continue. The lesson is to schedule by dependency and deadline, then measure whether the critical path actually improves.

## Shipped examples

**Destiny (2014).** Bungie moved most of the engine into a job graph, with fine-grained task and data parallelism. The talk describes limiting reliance on thread preemption. Graph edges make dependencies explicit and let the scheduler select runnable work as inputs complete. [Barry Genova, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

**Avatar: Frontiers of Pandora (2023).** Massive documents high-priority short tasks intended to complete mainly within a frame, alongside medium-priority long tasks that can span frames. Its frame example also time-slices adding objects to the world and overlaps certain graphics work. The separate work classes help express when each result is needed. [Joshua Simmons, GDC 2024, performance section](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/Simmons_Joshua_Upgrading_the_Snowdrop.pdf)

**Street Fighter 6 and Exoprimal (2023).** Capcom describes differentiated allocation strategies for game-loop performance threads and workers whose execution can tolerate delay. This is a reminder that worker granularity and priority also affect aggregate heap overhead and contention. [Capcom Open Conference Professional RE:2023](https://www.docswell.com/s/CAPCOM_RandD/ZXYDVM-RE2023)

## Techniques and practical checks

### Express real dependencies and measure the critical path

Declare which state a job reads or writes and when downstream work needs its output. Use explicit edges for dependencies and keep unrelated work runnable while a dependency waits. Instrument enqueue, start, completion, and wait time. Treat the longest chain required for the frame as the optimization target.

**Check.** Delay an upstream task in two branches, one on and one off the critical path. Confirm only dependent jobs wait and inspect which delay affects frame delivery.

### Partition work and merge results under an owner

Give jobs disjoint write ranges or have them produce local outputs. Combine results at a named phase. If order affects gameplay, sort by a stable key or define a deterministic reduction and tune numeric tolerance to the project.

**Check.** Run the same fixture with a single worker and several worker counts, including randomized completion order. Compare authoritative state and event sequences and look for simultaneous writes to shared data.

### Choose job sizes and priorities with data

Batch enough work to outweigh queue and scheduling costs. Keep the batch small enough to avoid a single long tail holding back a frame. Separate background jobs such as decompression from work needed for the next simulation step. Use budgets or fairness rules so work can eventually finish even when urgent work is constant.

**Check.** Sweep batch sizes on low- and high-core target devices. Record critical-path time, queue wait, throughput, and longest background wait. Verify random CPU spikes do not stall simulation or prevent critical assets from becoming ready.

### Make cancellation and thread handoffs explicit

Publish a job's result before signaling completion. Commit engine operations on the thread or phase that owns the target. If work can finish after unload, carry a scene or entity generation and discard a stale completion. Drain dependent work before reclaiming shared scratch or object storage.

**Check.** Force a job to finish during scene reload, rapid level switching, and game shutdown. Verify the active scene sees no stale result and that shutdown completes without an outstanding dependency or waiting worker.
