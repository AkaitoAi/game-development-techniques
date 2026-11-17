# Procedural generation

## Problem and design reasoning

**Problem.** Hand-authored content runs out of variation, or generated layouts produce unreachable goals and repetitive routes. Count invalid seeds and repeated structures before adding randomness.

**Design reasoning.** Rogue constrains rooms to coarse regions in source, and Brogue Community Edition adds loops after carving. The inferred priority is to reserve randomness for variation that can be checked against explicit guarantees. See the shipped examples below for sources.

Specify what changes per run and what every run must guarantee. A generator can assemble authored pieces, build geometry, or produce the state that supplies names, histories, and objectives.

## Shipped examples

**Rogue (1980).** In the preserved historical source, room generation divides the playfield into a coarse grid, then varies room dimensions and positions within its regions. This constrains the search space and spreads rooms across the floor while preserving local variation. [Historical Rogue room-generation source](https://github.com/Davidslv/rogue/blob/master/rooms.c)

**Brogue.** Creator Brian Walker presented the game's procedural level design at Roguelike Celebration. The published Community Edition source illustrates a staged version of the carve-and-connect approach. It carves the dungeon, then explicitly adds loops before transferring the result to the play map. Connectivity and route variation can be tuned at different stages. [Walker, Roguelike Celebration 2016](https://www.roguelike.club/schedule.html) · [Brogue Community Edition generator](https://github.com/tmewett/BrogueCE/blob/master/src/brogue/Architect.c)

**Caves of Qud (2024 full release).** Freehold Games describes a hybrid world where authored story runs through generated histories and simulated systems. Its developers' paper describes generating biographies as structured historical events and turning them into accounts and artifacts players can discover. The developers also discussed procedural generation and authorial voice at the International Roguelike Development Conference. This extends generation from level geometry to content whose references need to agree across the world. [Freehold Games paper](https://www.freeholdgames.com/papers/Generation_of_mythic_biographies_in_Cavesofqud.pdf) · [Jason Grinblat, International Roguelike Development Conference talk via press kit](https://cavesofqud.com/press-kit/) · [Developer and publisher store listing](https://store.steampowered.com/app/333640/Caves_of_Qud/)

## Techniques and practical checks

### Generate topology before placing detail

Place required nodes such as entrances, objectives, and exits on a graph. Assign gates and their prerequisite items before embedding the graph into geometry. Connect optional branches and loops after meeting required route constraints. If a graph cannot be embedded within the attempt budget, use a tested fallback.

**Check** reachability on the final playable geometry using the actual traversal rules, including keys consumed on use, one-way drops, character clearance, and movement abilities. Run that check on a saved corpus of seeds. Keep a failing seed and a minimal route trace whenever reachability breaks.

### Combine authored anchors with variable connectors

Give authored modules footprints, connector direction and size, supported elevation, and the kinds of play they support. Match those constraints before placing them. Reserve authored landmarks and place generated material around them. Use weighted selection with explicit limits to control repetition.

**Check** seams, collision clearance, sight lines, and route length at the joins. Review the longest and shortest generated routes and a sample chosen before looking at the outputs. Count repeat modules and verify that each required encounter remains legible in every valid approach.

### Make a run reproducible

Record the seed, generator version, and relevant content-set version. Give independent stages reproducible random streams derived from named inputs. Use stable iteration order where reproducibility matters. For old saves, retain enough resolved world state or generation compatibility to preserve already visited content.

**Check** the same seed across repeat launches and supported execution configurations. Compare semantic hashes of generated topology and identifiers. Modify an isolated decorative stage and confirm it leaves required routes and quests unchanged.

### Generate shared facts before generating their presentations

Represent places, people, events, rewards, and clues by stable IDs and explicit relationships. Build quest text, map markers, and descriptions from those facts. Require that generated clues resolve to available evidence and that objectives point to content present in the same generated world.

**Check** each reference resolves and each required clue is obtainable in the intended order. Inspect a sample for repeated phrasing, contradictory dates, and variants where different players discover evidence in different orders. Measure generation time and rejection rates alongside success.
