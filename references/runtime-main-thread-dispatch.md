# Runtime main-thread dispatch

## Problem and design reasoning

A background job may finish on a thread that cannot safely touch the UI, scene graph, or engine objects. Posting a callback solves thread affinity only when the recipient is still valid and the callback runs at a legal phase. In *Destiny*, Bungie's job graph and resource tracking validated reads, writes, creation, and destruction against safe points in the frame. This illustrates why completion time and legal publication time are separate concerns. [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

Microsoft's GDK task queue makes work and completion separate ports. A completion port can use manual dispatch so the game decides where callbacks run. Its immediate mode executes on the submitting thread, which makes assumptions about deferred execution unsafe. The dispatcher design below applies these documented contracts as recommendations for a game runtime. [Microsoft, Async task queue design](https://learn.microsoft.com/en-us/gaming/gdk/docs/features/common/async/async-task-queue-design)

## Techniques and practical checks

### Dispatch an owned result

Transfer a result payload with a clear release rule and a generation-checked recipient handle. Avoid queued closures that rely on borrowed worker buffers or raw pointers to replaceable scene objects. Validate the request identity on the receiving thread before committing the result.

**Check** delay dispatch, retire the recipient, and reuse its slot. Verify rejection of the old result and exactly one release of the transferred payload.

### Drain at an explicit phase

Name the point where the main thread accepts completions, such as before a simulation update or UI layout pass. Queue work submitted during the drain for the next documented drain unless same-pass execution is deliberate. Keep immediate completion behavior consistent with the public API contract.

**Check** post a completion from a worker and from inside another completion. Verify the documented order, thread, and phase, including a request that finishes synchronously.

### Bound callback cost and queue growth

Track queue age, pending payload memory, and execution time. Apply a per-frame budget and a policy for obsolete work. Coalesce replaceable updates by identity where appropriate while retaining mandatory terminal notifications and resource release.

**Check** release a burst of completions during a busy frame. Verify that frame cost stays within the configured budget and that critical completion latency and retained memory remain bounded.

### Close the dispatcher without a dependency cycle

Stop accepting requests for a retiring scope, settle pending completions, and retain cleanup machinery until its producers finish. A main thread waiting for a worker must not prevent the completion that the worker needs to finish. Specify what happens to posts that arrive after closure.

**Check** shut down with workers running, completions queued, and a callback that submits follow-up work. Verify deterministic rejection or completion, no deadlock, and no callback touching disposed state.

See [Game thread ownership](game-thread-ownership.md) for access rules and [Game thread backpressure](game-thread-backpressure.md) for overload policies.
