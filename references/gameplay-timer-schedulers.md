# Gameplay timer schedulers

## Problem and design reasoning

Delayed callbacks can run after their owner disappears, execute twice after a hitch, or change behavior when multiple deadlines coincide. The released *Quake* server checks an entity's next-think deadline against the current frame window, clears that deadline before invoking the callback, and checks whether the entity removed itself. Its source comments explicitly identify timing slack for ordinary think callbacks and stricter needs for moving push objects. [id Software, SV_RunThink in Quake](https://github.com/id-Software/Quake/blob/master/WinQuake/sv_phys.c)

Barry Genova's GDC 2015 talk describes the task and data parallelism used across *Destiny*. A timer scheduler that dispatches work into such an engine must define the thread and frame phase in which its callbacks may mutate gameplay. That connection is an architectural inference rather than a claim about Destiny's timer implementation. [Bungie, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Techniques and practical checks

- **Choose deadline semantics.** Give each timer an explicit clock domain, deadline, and repeat policy. For repeats, choose whether a hitch catches up every missed occurrence, skips missed periods, or performs a bounded catch-up. **Check** a one-second hitch, pause, slow motion, and resume. Verify invocation count and next deadline against the selected policy.
- **Bind callbacks to an owner generation.** Store a cancellable handle and an owner identity that can detect reused entity slots. Cancel owned work on teardown and validate identity before dispatch. **Check** destroy an actor before expiry, reuse its storage for a new actor, and advance time past the old deadline. Verify that the stale callback cannot affect the replacement.
- **Make dispatch order stable.** For equal deadlines, use a documented secondary key such as a monotonic insertion sequence. Queue scheduling and cancellation performed inside callbacks for a defined dispatch boundary. Place a budget on immediate rescheduling. **Check** callbacks that cancel each other, schedule another callback for now, and request the same deadline. Replay the trace and compare event order and work per tick.
- **Separate due work from legal execution.** Collect expired timers, then execute them in the gameplay phase that owns their state. If dispatch crosses threads, preserve cancellation and lifetime validation through the handoff. **Check** expire a timer during physics, a scene transition, and pending shutdown. Verify that mutation happens in the intended phase and that cancelled work does not execute after teardown.

For pause rules, read [Game clock domains](game-clock-domains.md). For broad scheduling dependencies, read [Game engine job scheduling](game-engine-job-scheduling.md).
