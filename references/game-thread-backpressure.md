# Game thread backpressure

## Problem and design reasoning

When a producer submits work faster than a game thread or its consumers can finish it, a queue turns a temporary slowdown into delayed feedback and retained memory. The critical timing question is how old a result may be before it loses value. A scene transition must complete in order, while a continuous preview can often replace a waiting request with a newer one.

In shipped *Destiny*, Bungie's rendering team identified render extraction as latency-sensitive because it blocked the next simulation tick. The team reduced render jitter using asynchronous buffer swaps where supported and latency recovery. Its broader engine talk describes pipelining system work across frames. **Design inference:** parallel work needs a bound on how far producers can run ahead of their consumers. The talk establishes the latency problem and the team's pipeline design. The policies below are implementation options. [Natalya Tatarchuk, Destiny rendering, GDC 2015](https://advances.realtimerendering.com/destiny/gdc_2015/Tatarchuk_GDC_2015__Destiny_Renderer_web.pdf) · [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Practical checks

- **Check** stall a consumer for several frames and measure queue bytes, depth, and oldest item age. Enforce a limit and record whether each full-queue submission waited, replaced a pending update, or was rejected.
- **Check** submit a burst of writes to one preview key alongside an ordered state transition. Verify that coalescing keeps the newest eligible preview and delivers the transition once in order.
- **Check** run expensive low-priority requests alongside a frame-critical request. Verify a reserved budget or priority mechanism meets the latter's deadline and gives background work a documented chance to progress.
- **Check** unload a scene with queued and in-flight requests, then reopen it and submit fresh work. Verify stale completions cannot affect the new scene, queue slots are returned, and no producer blocks a thread the consumer needs.
