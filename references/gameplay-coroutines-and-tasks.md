# Gameplay coroutines and tasks

## Problem and design reasoning

A gameplay sequence can suspend while waiting for time, animation, or a result, then resume after another action has taken control of the character. The suspended code still holds local variables, but its assumptions about the world may have expired. In the released *Celeste* player code, a state machine registers a dash coroutine alongside begin, update, and end callbacks. Scripted movement routines also yield while applying movement over successive frames. This makes coroutine execution part of an action's lifecycle. [Maddy Makes Games, released Celeste player code](https://github.com/NoelFB/Celeste/blob/master/Source/Player/Player.cs)

For *Destiny*, Bungie split portions of the main loop into job fibers within a graph and validated resource access at safe frame phases. That supports a second distinction: a readable sequence still needs an execution context whose data access is legal. The ownership protocol below is a portable recommendation, rather than a claim about either engine's exact coroutine implementation. [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Techniques and practical checks

### Bind the continuation to an action instance

Give each running action an identity that changes when the action is replaced. On resumption after an external wait, resolve the owner and validate both its generation and the action identity before changing gameplay state. State explicitly whether disabling an actor pauses the routine, ends it, or lets it continue.

**Check** suspend action A, replace it with B, then complete A's wait. Repeat after pooling and reusing the actor. Verify that A cannot change B's movement, state, or presentation.

### Declare the clock and resume phase

Choose simulation ticks, scaled presentation time, or wall time for each wait. Resume authoritative changes in the phase that owns them. Treat coroutine yields and task awaits according to the scheduler's actual contract, including completions that happen immediately.

**Check** complete the wait synchronously and asynchronously, then repeat with pause, slow motion, and a long frame. Verify the action commits in the intended phase and at the intended elapsed time.

### Keep child work and cleanup in the same scope

Track child routines that form part of the action. Define whether completion waits for them and how interruption releases acquired resources or temporary control overrides. Use a terminal path that restores owned flags even after an exception or cancellation. Release an override only if this action still owns it.

**Check** interrupt the parent before a child starts, while it waits, and while it completes. Verify one terminal outcome, no orphaned child, and restoration of input and animation control.

### Make suspension visible in diagnostics

Record owner, action identity, current wait, start tick, and terminal result. Put timeouts on waits that depend on an event that can disappear, with an explicit fallback. Measure live routine counts and continuation work in representative encounters.

**Check** remove the awaited target or suppress its completion event in a fixture. Confirm a bounded fallback, useful diagnostics, and no retained action after leaving the level.

See [Asynchronous task cancellation](asynchronous-task-cancellation.md) for stopping background work and [Game clock domains](game-clock-domains.md) for clock policy.
