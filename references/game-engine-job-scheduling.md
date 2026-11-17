# Game engine job scheduling

## Problem and design reasoning

Frame completion depends on the work that gates presentation. A scheduler can show high total worker utilization while the frame waits on one long dependency chain. Naughty Dog redesigned its job system for *The Last of Us Remastered* so jobs could yield while waiting on a counter, with ease of use for gameplay programmers among its stated goals. Bungie's *Destiny* talk describes a job graph for most engine work and runtime validation of resource access and lifetimes. The choice between yielding fibers and explicit continuations depends on the workload and tooling. A transferable principle is to show what blocks the next stage and keep workers available to execute its prerequisites. [Christian Gyrling, Parallelizing the Naughty Dog Engine Using Fibers, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Gyrling_Christian_Parallelizing_The_Naughty.pdf) · [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Techniques and practical checks

### Expose the critical path

Record when jobs become eligible, enter a queue, run, and complete. Tag them with frame, dependencies, and thread affinity. Follow the dependency chain ending at a missed deadline and use that trace to decide where work should move or overlap.

**Check** reproduce a slow frame and identify which predecessor kept rendering or simulation from proceeding. Compare idle and queue-wait time with execution time before and after rescheduling it.

### Enforce dependencies and resource lifetimes

Declare the job's inputs and outputs, identify its owner, and schedule readers after producers. Release shared buffers after their final consumers complete, including on cancellation. Give main-thread-only stages explicit handoffs rather than permitting workers to call those APIs directly.

**Check** run with randomized ready-job order and change scenes while work is in flight. Look for stale buffers, callbacks into destroyed scenes, and writes after cancellation. Add assertions around resource access in development builds.

### Keep waiting work off the worker pool

Use dependency continuations, a yielding scheduler, or a separate executor for unavoidable blocking work. Avoid holding shared locks while waiting on downstream jobs. Make affinity requirements explicit so a job queued to the main thread cannot be hidden behind an unserviceable wait.

**Check** reduce the pool to one worker and exercise a job that creates child work and awaits completion. Repeat during a slow storage read. Verify that runnable prerequisites execute and that shutdown drains or cancels the work within its timeout.

### Control scheduler overhead and background work

Group tiny work items into batches, but bound the size of a long batch when urgent work shares the pool. Cap optional jobs during heavy frames and record their backlog so starvation is visible. Tune worker counts and batch thresholds on target hardware.

**Check** measure a dense scene while background streaming and compilation are active. Compare queueing cost, frame-time percentiles, and backlog age across batch sizes. Confirm that the frame meets its target and background work still finishes.
