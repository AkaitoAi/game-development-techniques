# Game mechanics

## Problem and design reasoning

**Problem.** Players misunderstand cause and effect, a dominant action makes choices irrelevant, or interacting rules produce outcomes that are impossible to anticipate.

**Design reasoning.** Nintendo documents sketching early player actions and using level geometry to teach. Into the Breach's designers describe previewing outcomes. A design inference is to expose the rules that matter for a decision and then test their combinations for unintended dominance and useful depth.

A mechanic is a rule that turns a player action or game event into a change in game state. Designing one means choosing what the player can do, what information they can act on, and what consequences give the choice weight.

## How shipped games use rules

- **Super Mario Bros. (1985)** teaches the mushroom through the geometry of World 1-1. The first pipe reflects a mushroom that rolls past Mario, giving the player another chance to collect it and learn what it does. The developers sketched possible player actions on paper and continued adjusting the level late in production. [Nintendo developer interview](https://iwataasks.nintendo.com/interviews/wii/nsmb/1/4/)
- **Street Fighter II (1991)** illustrates how depth can emerge where rules overlap. Director Akira Nishitani recalls that an uppercut-to-Shoryuken combo was unplanned. A successful interaction can become part of the game even when the team discovers it while testing, provided its consequences work across matchups. [Nishitani's 1991 interview, English translation](https://shmuplations.com/streetfighterii/)
- **Into the Breach (2018)** makes enemy attacks visible before resolution and gives player attacks predictable outcomes. It puts the uncertainty in the situation the player faces and lets the player reason about how to save objectives, sometimes at the cost of a clean victory. [Justin Ma interview](https://www.gamedeveloper.com/game-platforms/road-to-the-igf-subset-games-i-into-the-breach-i-) · [Matthew Davis's design postmortem](https://www.gdcvault.com/play/1025772/)
- **Street Fighter 6 (2023)** groups multiple offensive and defensive techniques under one Drive Gauge. Spending on one response changes the resources available for another. Capcom also provides a Modern input scheme alongside Classic, giving players another way to reach combat decisions. [Capcom's battle system overview](https://news.capcomusa.com/2022/06/02/street-fighter-6-redefines-the-genre-in-2023/)

These examples trace a progression in what designers can make explicit. World 1-1 teaches through physical placement. Street Fighter II exposes interactions through repeated play. Into the Breach makes impending consequences part of the interface, while Street Fighter 6 makes resource trade-offs and inputs inspectable. Each approach assumes that players can learn the relationship between what they do and what happens next.

## Techniques

### Specify one decision as a state transition

Write down the action, eligibility, cost, consequence, and the next legal decision. For a push on a grid, define which units can push, what happens when a destination is occupied, when movement resolves, and whether the push is spent on failure. Prototype a single decision with a few board states. **Check:** For each fixture, predict the complete next state and confirm the preview and resolution agree, including blocked moves and simultaneous events.

### Teach through a recoverable encounter

Give the player one readable opportunity to try the mechanic, room to recover from a plausible mistake, and a second situation where the same rule solves a new problem. For a push, start beside a clear lane, then ask the player to move a crate off a switch. **Check:** Observe blind first attempts, record where help was needed, and ask the player to predict whether the second push will work before they try it.

### Put information where a choice happens

Show the consequence next to the action or object it concerns. For simultaneous threats, show targets and resolution order and preview the consequences of a selected move. Derive preview and execution from the same resolution rules where possible. **Check:** Compare what players predict against what triggers in scenarios with overlapping targets, displacement, and chained reactions. Check that identical indications imply identical rules.

### Test for competing strategies

Combine a few mechanics in a repeatable test scenario. Vary one cost, reward, or enemy at a time. Log action sequences and outcomes as well as completion rates. **Check:** Try the obvious repeatable move, a resource-hoarding strategy, and a high-risk route against the same fixtures. Inspect any strategy that dominates across contexts, and decide which situations should make a different choice worthwhile.
