# Stealth game design

## Problem and design reasoning

**Problem.** In a fictional stealth encounter, players report that detection feels arbitrary or that a recoverable alert feels indistinguishable from failure. Inspect what the interface communicates when an NPC changes state.

**Design reasoning.** Klei describes adapting stealth conventions to support choices in a side-scrolling view. An inferred design constraint is to communicate enough of the game's sensing rules for players to predict broad outcomes and learn from a failed attempt. See the shipped examples below for sources.

## Shipped examples

**Mark of the Ninja (2012).** Klei lead designer Nels Anderson described adapting stealth to a side-scrolling view around player choice. The change in perspective called for reexamining which conventions served that choice. Start by deciding what information the player needs to compare the actions available in your camera and interface. [Nels Anderson, GDC 2013](https://www.gdcvault.com/play/1017791/Of-Choice-and-Breaking-New)

**Invisible, Inc. (2015).** Klei developed a procedural approach to stealth levels. Designer James Lantz's talk traces that approach and its design problems from conception to release. A generated encounter needs constraints for the choices it offers, in addition to constraints for physical connectivity. [James Lantz, GDC 2015 Independent Games Summit](https://www.gdcvault.com/play/1021920/Designing-Procedural-Stealth-for-Invisible) · [Klei game overview](https://www.klei.com/games/invisible-inc/)

**Hitman World of Assassination (2023 compilation).** IO Interactive describes disguises, alternate starting locations, and mission ratings that reflect the player's approach. These give repeat players different choices before entering the same authored space and feedback on the run afterward. [IO Interactive, game features](https://ioi.dk/hitman/game-features)

## Techniques and practical checks

### Give detection an observable state

Define discrete states such as unaware, investigating, and alerted, or use a continuous suspicion value with documented thresholds. Connect those states to cues that fit the camera and genre. Keep the cue and the game rule driven by the same state. If the design hides a threshold, still signal the direction of change so players can attribute an outcome.

**Check** by crossing a detection boundary slowly and quickly, entering and leaving sensor range in one update, and receiving signals from multiple observers. Compare the visible and audible cues with the debug state trace.

### Author readable alternatives inside the level

Describe the fictional interaction rules for each route or role, including the affordances that tell players which rules apply. Give alternatives different costs in game resources, time, or exposure to an abstract detection meter. Teach a rule in a contained encounter before combining it with other rules.

**Check** with first-time playtesters. At a fork, ask them what could happen on each route before they commit. Compare predictions with outcomes and record which cues drive mistaken choices.

### Design recovery as a playable state

Decide which conditions end a run and which allow it to continue under altered objectives, costs, or scoring. Give the current state a clear presentation and provide an explicit exit from temporary penalties. Keep objective and checkpoint logic consistent with alarm state.

**Check** trigger detection before a checkpoint, at an objective transition, and immediately before the exit. Reload each checkpoint and verify the next objective and alert state. Inspect repeated state changes for stuck cues or repeated rewards.

### Validate selections and transitions in generated levels

Represent scripted gates, detection rules, and objective order as part of a level test fixture. Evaluate seeds with a simple rules-aware explorer to confirm at least one legal completion path from each supported start. Where choice is a design goal, sample how many materially different paths survive the constraints.

**Check** test many fixed seeds, preserve failing seeds as regressions, and play a sample of valid generations. Record impossible starts, forced outcomes, and routes that look available while violating a hidden rule.
