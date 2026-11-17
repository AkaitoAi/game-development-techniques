# Enemy design

## Problem and design reasoning

**Problem.** Enemies overwhelm the player through accidental overlap, or attack outcomes become hard to read when several patterns run together.

**Design reasoning.** Bungie describes encounter task priorities and capacities. Housemarque describes iterating Hyperion's patterns and validating an unharmed run. The reasoning is to author opportunities and limits at encounter scale, then verify combined readings rather than tuning each enemy in isolation.

Design each enemy around the decision it asks the player to make, such as changing position, choosing when to interrupt, or prioritizing one threat over another. Give that decision a readable cue and a response window the player can learn.

## Shipped examples

**Halo 3 (2007).** Bungie's encounter system organized tasks into a priority tree. Tasks had capacities and activation rules. Squads moved between tasks as those rules changed, allowing designers to specify the desired distribution of a battle, such as assigning squads to fallback positions. [Damian Isla, Bungie GDC slides](https://aarmstrong.org/files/gdc2008/Building%20a%20Better%20Battle%20HALO%203%20AI%20Objectives.pdf)

**Returnal (2021).** Housemarque layered Hyperion's projectile patterns with a consistent color language for different forms of pressure. Its developers describe iterating the combined patterns and requiring a damage-free run to demonstrate that a practiced player could read and avoid the encounter. [Housemarque's Hyperion development account](https://blog.playstation.com/2021/05/28/returnal-the-making-of-that-unforgettable-hyperion-fight/)

These examples show two scales for tuning difficulty. Encounter tasks organize where the pressure comes from, while attack patterns determine how the player reads and responds to it moment by moment. A small game can retain both ideas with a few authored roles and explicit attack windows.

## Techniques

### Define attack commitments and responses

Author the wind-up cue, active interval, and recovery for each attack. Choose which responses the attack permits and when targeting commits. Store damage and collision windows in gameplay time and align animation and effects to those events. Give essential cues a shape, motion, or sound distinct enough to read without color alone.

**Check** responses one tick before and after each boundary. Replay with audio muted, effects reduced, and the camera near obstacles. Check for damage before the cue, hits after recovery begins, and accidental multiple hits.

### Combine enemy roles using limits

Give each role a job, such as claiming space or forcing movement. Use encounter slots or a shared capacity to limit concurrent high-pressure actions. Grant slots only when an actor is ready and release them on recovery, interruption, disable, or unload. Give waiting actors visible secondary behavior.

**Check** each role alone, then in pairs and full groups. Record overlapping active windows, actor wait time, and available paths. Repeat when the player enters from an unusual direction or removes one role early.

### Make escalation observable

Specify what changes between phases, such as a new movement rule, a shorter recovery, or a new pattern alongside a familiar one. Choose a consistent phase cue. Give transition logic explicit rules for surviving hazards and one-shot triggers.

**Check** the transition while an attack is active, after a large state change, and after checkpoint reload. Practice the intended responses at the supported difficulty levels. Treat a damage-free test as appropriate when every threat is intended to be avoidable.

### Diagnose outcomes through replays

Log chosen actions, target selection, cooldowns, interruptions, and damage causes alongside a replay or a short video. Tie each adjustment to a player-facing symptom and change one timing or selection variable at a time.

**Check** whether novice and experienced players can explain the damage they took and improve on a repeat attempt. Inspect long sequences without a safe recovery and compare completion, retry, and response timing on equivalent runs.
