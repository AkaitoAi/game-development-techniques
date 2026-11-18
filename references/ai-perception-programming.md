# AI perception programming

## Problem and design reasoning

An actor that reads a hidden target's live position can react with information it never observed. An actor that scans every object every tick can consume the frame budget. In *Halo 2*, Bungie stored perceived object state separately from actual world state, allowing an actor to retain a last-known location. The separation supported believable mistakes and target-specific behavioral memory. [Damian Isla, Handling Complexity in the Halo 2 AI, GDC 2005](https://www.gamedeveloper.com/programming/gdc-2005-proceeding-handling-complexity-in-the-i-halo-2-i-ai)

The released *Quake* implementation rotates candidate clients and uses the potentially visible set as an inexpensive rejection test. Its candidate is only potentially visible, so this test alone cannot establish unobstructed sight. The programming recommendations below combine bounded candidate work with explicit evidence records. [id Software, Quake perception candidate code](https://github.com/id-Software/Quake/blob/master/WinQuake/pr_cmds.c#L650-L753)

## Programming techniques and practical checks

### Store observations with their context

Keep target identity, sensor kind, observed position, observation time, and validity in actor-owned memory. Define whether another actor's report is trusted and how it changes confidence. Let behavior query that knowledge store through explicit access rather than reading hidden live transforms.

**Check** hide a target and move it elsewhere. Trace the actor's chosen destination and evidence timestamp. Verify investigation uses the allowed knowledge until new evidence arrives.

### Stage candidate and visibility work

Use a spatial query or visibility region to find candidates, then apply range, facing, obstruction, and gameplay-specific rules. Budget expensive tests across actors while giving significant nearby stimuli a bounded response time.

**Check** compare results against an exhaustive small-scene oracle, including thin walls and a target near the range limit. Measure maximum observation age under a crowded-scene load.

### Give evidence a lifecycle

Define refresh, expiry, and explicit invalidation for each sense. Use stable target handles so a recycled entity slot cannot inherit another object's history. Separate loss of current sight from deletion of remembered information.

**Check** destroy a target, reuse its slot, and cross a memory timeout during a pause or streamed-cell unload. Confirm the intended clock drives expiry and stale evidence never binds to the new actor.

### Resolve results at a controlled boundary

If sensor queries run asynchronously, attach sensor and target revisions to the result. Merge completed observations in a defined order and discard results whose owners have changed. Expose sensor volumes, last-known positions, and rejection reasons in a debug view.

**Check** finish a sight query after a teleport, faction change, or sensor disable. Verify the stale result cannot restore pursuit and that the trace shows why it was rejected.

See [Game AI](game-ai.md) for decision architecture and [Spatial indexing](spatial-indexing.md) for candidate queries.
