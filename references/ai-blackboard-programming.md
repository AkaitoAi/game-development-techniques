# AI blackboard programming

## Problem and design reasoning

AI tasks need shared context, but an unstructured collection of values can preserve a destroyed target, mix observations from different ticks, or leave one task interpreting another task's temporary state. Santa Monica Studio's *Preparing for Ragnarök* presentation shows a blackboard alongside behavior-tree debugging, including player, position, movement, and context-action values. This documents the value of inspecting the context that produced a decision in the shipped *God of War Ragnarök*. [Santa Monica Studio, GDC 2023 slides and notes](https://media.gdcvault.com/gdc2023/Slides/PreparingForRagnarokWithNotes.pdf)

Valve's Source SDK AI memory records entity knowledge with timestamps and positions. The design inference is that shared AI facts should carry enough identity and time information for consumers to judge whether they remain usable. A memory record is a useful source of observations for a blackboard even when the implementation uses separate data structures. [Valve Source SDK, ai_memory.h](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/ai_memory.h)

## Techniques and practical checks

### Give each key a typed contract

Define the key's type, meaning, scope, default, and writer. Separate a perceived target from a selected target and a requested destination from a completed movement result. Keep stable entity handles rather than unsafe object references. Use an explicit missing state when a zero value is meaningful.

**Check** a task starting without a target, a target at the world origin, and a destroyed target. Verify each case produces the intended branch and that invalid handles cannot resolve to a reused actor.

### Track validity and ownership

Attach update time, source, and revision where consumers need freshness. Let perception own observations and let decision tasks own commitments. Clear temporary keys when their owner exits or is cancelled. Persist only the context required by the intended save or network model.

**Check** a pursuit interrupted by another task, followed by actor reuse or reload. Inspect which keys survive and verify that a new task never inherits an unrelated commitment.

### Publish coherent context

Evaluate dependent values from one consistent snapshot or publish a versioned set after computing it. Queue notifications until the update is complete so an observer cannot react to half-written target and position data. Bound repeated task restarts when many keys change at once.

**Check** a target switch during a perception update and a rapid burst of stimuli. Record the context revision used for each decision and confirm every action uses a compatible target and position.

### Make decisions inspectable

Provide a debug view for current values, writer, age, and the last meaningful change. Trace why an action selected or rejected a key. Prefer filtered traces for the chosen actor so high-frequency sensor updates remain readable.

**Check** reproduce a stale-target bug with a fixed input or scenario trace. Verify the tool shows when the observation became stale, which task consumed it, and the reason the fallback did or did not run.

See [Behavior tree programming](behavior-tree-programming.md) for task lifecycle and [AI perception programming](ai-perception-programming.md) for observation validity.
