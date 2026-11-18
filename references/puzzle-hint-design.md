# Puzzle hint design

## Problem and design reasoning

**Problem.** A player needs help identifying a relevant object or relationship, while an unsolicited solution removes the discovery they came to experience. A hint can also become false after the puzzle changes.

**Developer reasoning.** Daniel Wewerinke's GDC 2024 *Relic Ruins* presentation examines playtesting and iteration for Horizon Forbidden West and Burning Shores. Its lessons include the absence of a hint opt-in/out choice. Santa Monica later added reduced companion hint frequency to God of War Ragnarök. Together these documented cases motivate treating assistance as an adjustable information service. The workflows below are portable recommendations. [Wewerinke's talk](https://www.gdcvault.com/play/1034875/Relic-Ruins-Creating-Environmental-Puzzles) · [Guerrilla's slides](https://www.guerrilla-games.com/media/Team/Daniel%20Wewerinke%20-%20GDC24%20-%20Relic%20Ruins%20-%20Creating%20Environmental%20Puzzles.pdf) · [Santa Monica's announcement](https://blog.playstation.com/2024/09/18/god-of-war-ragnarok-brings-a-host-of-new-features-to-pc-available-tomorrow/)

## Techniques and practical checks

### Author information in stages

For each puzzle state, record the missing observation, useful relationship, and concrete next action. Write separate hint tiers for those needs. Avoid disclosing later dependencies in an early observation hint. Require an explicit request before escalating where discovery is central. Pair spoken hints with captions and an optional repeat action.

**Check** each tier with unfamiliar players. Ask what they now believe and observe their next action. Reject a hint that merely repeats what they already know or reveals more than its tier promises.

### Define when assistance is eligible

Track meaningful progress events such as discovering an object or completing a dependency. Pause eligibility during menus, combat, dialogue, and time away from the puzzle. A long interval is evidence to offer help, with stronger assistance remaining adjustable. Separate solving difficulty from execution difficulty. God of War Ragnarök documents puzzle timing and puzzle aim assistance independently. [Official accessibility settings](https://www.playstation.com/en-us/games/god-of-war-ragnarok/accessibility/)

**Check** a thoughtful player who explores slowly, a player who misses an affordance, and a player who knows the solution but cannot execute quickly. Confirm each receives an appropriate option and can decline it.

### Validate hints against live state

Associate queued hints with a puzzle instance, state revision, prerequisites, and completion condition. Revalidate before playback. Cancel stale hints after moving an object, resetting, leaving the area, or loading a save. Allow required story dialogue to defer assistance without stacking multiple hints.

**Check** solving the step immediately before playback, resetting during a hint, reentering from another route, and loading an intermediate save. Confirm the remaining advice describes an achievable action.

### Measure assistance quality

Record offers, requests, tier, subsequent progress, and abandonment without assuming every unused hint is a failure. Review observation sessions alongside those counts. Deliver an authoring table linking each tier to its state predicate and expected learning outcome. Use [puzzle design](puzzle-design.md) for rule teaching and solvability.

**Check** whether requesting a hint leads to independent progress. Inspect repeated requests and immediate departures individually, and retest revised wording before changing global timing.
