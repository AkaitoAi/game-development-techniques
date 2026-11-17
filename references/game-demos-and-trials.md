# Game demos and trials

## Problem and design reasoning

A public demo has to represent the game before players reach the point where they can judge its appeal. Copying the opening may leave its distinctive mechanics outside the sample. Handcrafting a separate slice adds production work and can teach procedures that differ from the full game. Strange Scaffold producer Amanda Farough reported that preparing the Steam Next Fest demo for *Teenage Mutant Ninja Turtles: Tactical Takedown* required custom in-game gating on a tight production schedule. A practical inference is to budget a demo as its own supported build, including content boundaries and handoff, when those features differ from the release. [Amanda Farough, GDC 2026 production retrospective](https://media.gdcvault.com/gdc2026/Slides/Farough_Amanda_BuildingACriticallyAcclaimedLicensedGameIn18Months.pdf)

## Shipped-game examples

- **Dragon Quest XI S (2019)** sampled a substantial opening in 3D mode, including early quests and crafting. Nintendo stated that the player could carry their save into the full release. [Nintendo demo announcement](https://www.nintendo.com/jp/topics/article/6633ad58-b8fd-11e9-b641-063b7ac45a6d)
- **Final Fantasy XVI (2023)** paired an importable prologue with a separate, repeatable battle sample unlocked after completion. The battle demo exposed a wider set of abilities and accessories while its progress stayed in the demo. This shows how a team can offer continuity and demonstrate later mechanics with two different progression policies. [PlayStation demo announcement](https://blog.playstation.com/2023/06/11/final-fantasy-xvi-demo-launches-tomorrow/)

## Techniques and practical checks

### Define what the player can evaluate

Choose a finished sequence that introduces the controls, demonstrates a distinctive mechanic, and reaches a satisfying decision or outcome. If that mechanic arrives late in the base game, consider a separate challenge with a compact loadout and a short explanation of its new actions.

**Check** a cold start with players unfamiliar with the game. Measure time to the first meaningful choice and ask which mechanics they expect to use in the full game. Compare their answers to the shipped feature set.

### Make the content boundary explicit

Choose a chapter endpoint, content boundary, or timed-trial rule. For a timer, specify which contexts count, including pauses and suspended sessions. Reuse the full game's rules where they apply and isolate trial-specific limits in one place.

**Check** the final objective, branching routes, replay, skipping, and quitting before the boundary. For a timed trial, suspend and resume near expiry. Confirm progress is handled as advertised and the ending appears once.

### Test save transfer as a release path

Document which versions, profiles, and modes support import. Keep temporary challenge progress separate from importable campaign saves. Present the selected save and expected resume location before applying it, then retain the original until the import succeeds.

**Check** import from a published demo build into the release candidate. Include an existing full-game save, a profile mismatch, and a retry after interruption. Verify the expected checkpoint and state without replacing unrelated slots.

### Maintain the public build

Give the demo its own installation smoke test and known supported content version. Keep the last supported save format and player-facing transition text aligned with changes in the main game. End with a clear explanation of what is available next.

**Check** fresh installation, upgrade from the previous demo, offline startup where supported, and transition into the full game on each distribution path. Validate the actual downloaded builds and their endpoint messages.
