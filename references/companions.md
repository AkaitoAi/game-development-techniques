# Companion design

## Problem and design reasoning

**Problem.** An ally blocks a corridor, loses the player, or takes control of an objective the player meant to handle.

**Design reasoning.** Irrational's Elizabeth postmortem describes distinct needs to lead, follow, and switch between scripted and dynamic behavior. Pawn commands suggest a useful complementary inference. Give assistance an explicit handoff so the player can accept or interrupt it.

A companion needs a movement policy, a set of useful actions, and a way to yield control to the player. Define how those responsibilities change across travel, danger, dialogue, and scripted scenes.

## Shipped examples

**BioShock Infinite (2013).** Irrational Games describes Elizabeth as needing to lead and follow, switch between dynamic and scripted behavior, and stay engaging over extended play. Its AI postmortem treats those responsibilities as part of the architecture rather than a single follow behavior. [John Abercrombie, Irrational Games GDC postmortem](https://www.gdcvault.com/play/1020831/Bringing-BioShock-Infinite-s-Elizabeth)

**Dragon's Dogma 2 (2024).** Capcom's manual documents Pawn command inputs and an explicit way to accept a suggestion, such as an offer to guide the player or help with a task. Inclinations expose differences in Pawn behavior. This gives companions opportunities to act on what they know while leaving the decision to accept a suggestion with the player. [Capcom Pawn command manual](https://game.capcom.com/manual/dd2/en/ps5/page/4/7) · [Capcom Pawn inclination manual](https://game.capcom.com/manual/dd2/en/ps5/page/4/2)

Both cases make role changes part of the design. A companion can lead when the player accepts help, yield when the player changes course, and return to ordinary travel when the reason to lead ends.

## Techniques

### Follow relative to the route

Choose a reachable follow position with room for the player to turn, interact, and reverse direction. Place companions to the side or behind in narrow spaces and spread them when there is clearance. Use separate thresholds for catching up and slowing down. At traversal links, queue actors and release the queue when the link becomes invalid.

**Check** with sprinting, repeated reversals, platforms, one-way drops, ladders, and interactable doorways. Track how long the companion blocks the route or stays out of useful range. Test regrouping after fast travel and reload.

### Make assistance explicit and cancelable

Give commands and suggestions a target, eligibility rule, and cancel condition. Acknowledge accepted commands immediately. For guidance, cue the destination before moving, then check whether the player can follow. Let critical player actions cancel or outrank suggestions.

**Check** by accepting a suggestion, replacing it, walking away, and removing the destination mid-task. Confirm that reservations and state clear once, then verify that the next command works without a restart.

### Decide how companions share risk

Set clear rules for whether companions can trigger detection, block action space, use limited resources, or become incapacitated. Choose a recovery rule for each failure. If an authored scene replaces ordinary AI, transfer ownership explicitly and reconcile progress before restoring ordinary behavior.

**Check** in narrow arenas, stealth routes, and interrupted conversations. Test detection, downed recovery, and death or separation according to the game's rules. Verify no companion state alone can deadlock required progression unless the game gives a clear fail or retry path.

### Give incidental behavior a budget

Bind remarks and gestures to relevant world facts and let urgent gameplay cues take priority. Apply repeat limits and retain only the recent history needed to avoid repetition. Keep travel, incapacitation, and relationship state distinct so a blocked route does not accidentally reset narrative progress.

**Check** by waiting in one location, revisiting it after state changes, and moving past several points of interest quickly. Log repeated or interrupted remarks and ask players which actions felt helpful, out of place, or unclear.
