# Game clock domains

## Problem and design reasoning

A pause, slowdown, or rewind changes some durations while other work still needs to progress. A countdown that receives frame time by convention can accidentally keep advancing through pause, or a networking timeout can freeze when gameplay stops. A fixed timestep defines how a simulation advances. A clock domain defines what a timestamp means and what can change its rate or direction.

Jonathan Blow's *Braid* talk describes reproducing the scene at stored times because rewind is central to the puzzle rules. Timothy Ford's *Overwatch* talk describes a networked simulation designed for responsive multiplayer and authoritative outcomes. The examples motivate different timing policies. The following division into simulation, presentation, real elapsed time, and external deadlines is an architecture recommendation derived from those needs. [Jonathan Blow, The Implementation of Rewind in Braid, GDC 2010](https://www.gdcvault.com/play/1012210/The-Implementation-of-Rewind-in-Braid) · [Timothy Ford, Overwatch Gameplay Architecture and Netcode, GDC 2017](https://gdcvault.com/play/1024001/-Overwatch-Gameplay-Architecture-and)

## Practical techniques

### Give each duration an owner

Use simulation ticks or scaled game time for rules that obey pause or slowdown. Give presentation routines an explicit pause policy. Use monotonic elapsed time for process-local operations such as timeouts. Use an authoritative absolute timestamp for external dates or deadlines that must survive restart. Keep tick counts, elapsed values, and timestamps typed or clearly tagged.

**Check** pause during a cooldown while a menu transition and a network timeout are pending. Verify each progresses or freezes according to its design and that changing the device's date does not alter the elapsed-time timeout.

### Specify rate changes and discontinuities

Advance scaled time from elapsed time and the scale for each interval. Define what rates of zero, resume, seek, or rewind mean at their boundaries. Restore historical simulation state when backward integration would be unsafe. Give timer queues an explicit policy for overdue work after a jump.

**Check** switch from normal speed to slow motion, pause, resume, and seek. Put expirations on either side of each boundary and verify event order and delivery count.

### Convert time at subsystem boundaries

Map ticks to sampled presentation time using an explicit anchor and tick duration. Maintain an estimate of remote clock or tick offset only where synchronization needs it. Apply corrections to presentation without moving authoritative gameplay deadlines based on display smoothing.

**Check** inject delayed updates and a render hitch. Verify that a confirmed action stays on its authoritative tick while interpolation recovers and the UI does not briefly display negative remaining time.

### Separate authoritative side effects from replayable events

Attach ticks and stable occurrence IDs to effects that may be revisited during rollback or rewind. Define which state restores from a snapshot, which cosmetic actions can replay, and which irreversible operations run only once after confirmation.

**Check** restore a snapshot immediately before a timed reward and replay past it twice. Verify that each replay produces the intended simulation state and presentation while a persistent grant is recorded once.

## Countdown and persisted deadline boundaries

For a finite nonnegative countdown, subtract the accepted elapsed interval before testing expiry. Clamp the presented value to zero and commit the stopped state before delivering the terminal event. When the display rounds upward, use ceiling so positive fractions display one second and zero displays zero. Establish precedence when victory and timeout arrive in the same step.

```text
if running:
    remaining = max(0, remaining - elapsed)
    display(ceil(remaining))
    if remaining == 0:
        running = false
        deliver_expiry()
```

Test just below, exactly at, and beyond expiry, plus zero at entry and subsequent updates. Confirm expiry fires once. An arithmetic model proves its branch behavior. In Unity, record timeScale and maximumDeltaTime before describing a large injected elapsed interval as a reproduced frame hitch. Pause and slowdown tests should use the clock the gameplay rule owns.

For an offline deadline that survives restart, sample the current clock once and validate the stored timestamp's format and range. Compare current and stored values before subtraction, especially with unsigned types. A backward clock needs an explicit policy, such as zero credited elapsed time, and an untrusted device clock provides limited assurance. Use the project's existing time authority when available. Define missing and corrupt-save recovery separately from a valid unexpired claim.

Test backward time, exactly-at-expiry, offset changes, malformed persisted timestamps, restart, and duplicate claim attempts. Match the test to an active entry flow before attributing player impact.
