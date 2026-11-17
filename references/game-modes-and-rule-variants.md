# Game modes and rule variants

## Problem and design reasoning

A variant can reuse movement, maps, or characters while changing what counts as progress and when a match ends. Rules scattered across spawning, scoring, UI, and persistence make it easy for a variant to inherit incompatible assumptions. Blizzard's Stadium designers describe keeping the core feel of Overwatch while introducing new mechanics and strategic choices. A transferable inference is to make the match rules an explicit contract around shared gameplay actions. [Blizzard, GDC session](https://www.gdcvault.com/play/1035697/Designing-Stadium-Crafting-a-New)

## Shipped examples

- **Rocket League (2015).** Psyonix introduced Mutators as preset and custom settings that change match rules. The studio explicitly connected the feature to its team's history with Unreal Tournament modes and mods. This model gives players bounded ways to remix a familiar match. [Psyonix announcement](https://www.rocketleague.com/news/mix--match--and-mutate-this-november)
- **Overwatch 2 Stadium (2025).** Blizzard's launch version used a best-of-seven structure, a hero chosen for the whole match, and purchases and powers between rounds. Those choices add planning across rounds while characters still act through the game's recognizable combat verbs. [Blizzard launch guide](https://overwatch.blizzard.com/en-us/news/24188046/enter-the-stadium-get-ready-to-battle/) · [Blizzard, GDC session](https://www.gdcvault.com/play/1035697/Designing-Stadium-Crafting-a-New)

## Techniques and practical checks

### Give each ruleset an explicit lifecycle

Define phases such as setup, play, score resolution, intermission, and completion. Identify which state persists between rounds and which state resets. Have the match authority resolve scoring and terminal conditions with a documented tie policy.

**Check** a score and time expiry on the same tick. Verify the recorded winner, rewards, and summary all agree. Repeat the terminal event and confirm that the round closes once.

### Compose bounded modifiers

Describe each mutator's scope, legal range, dependencies, and incompatibilities. Define which layer takes precedence among the base mode, preset, and host options. Freeze the resolved settings when the match starts and carry the resolved settings into replays or saves that depend on them.

**Check** a supported combination, an incompatible pair, and an old preset containing a retired option. Verify that the match either starts with the displayed rules or explains what must change.

### Keep the mode contract visible to content and players

Declare required map features such as spawn sets, objective sockets, and navigation links. Present the active objective and progress using state supplied by the ruleset. Use the same mode identifier for content validation and selection screens.

**Check** a map missing a required objective and a late join to an active match. Confirm that selection rejects the incompatible map and that the joining player sees the current objective, score, and phase.

### Verify progression across rule boundaries

Decide whether a variant shares unlocks, ranks, or reward rules with the base mode. Scope match-end bookkeeping to a unique match result and define how abandon, rematch, and cancellation affect it.

**Check** winning, leaving, reconnecting during a round break, and rematching with a different ruleset. Verify the correct resets and that only eligible results reach progression.
