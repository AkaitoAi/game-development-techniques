# Quests and interactive narrative

## Problem and design reasoning

**Problem.** A quest works on its expected path but fails when players arrive early, use different abilities, or resolve objectives in another order. Check for contradictory dialogue and orphaned objectives.

**Design reasoning.** The Coles describe sharing a main story across classes while providing different puzzle solutions. An inferred trade-off is to spend branching effort where it expresses role or consequence, using common milestones where the paths can safely converge. See the shipped examples below for sources.

## Shipped examples

**Quest for Glory: So You Want to Be a Hero (1989).** Designers Lori and Corey Cole describe a shared main storyline with many puzzles supporting different skills or spells for different classes. A common destination can keep the story manageable while the means of getting there expresses the role the player selected. [Lori and Corey Cole interview excerpt](https://adventuregamers.com/article/lori-and-corey-cole-an-excerpt-from-the-art-of-point-and-click-adventure) · [The Coles' Sierra newsletter retrospective](https://www.mocagh.org/sierra/sierranews-summer91.pdf)

**The Witcher 3: Wild Hunt (2015) and Cyberpunk 2077: Phantom Liberty (2023).** At Taipei Game Developers Forum, quest director Paweł Sasko used examples from both games and Cyberpunk 2077 to explain how CD Projekt Red coordinates narrative production across departments and designs nonlinear quests. Treat the playable sequence and the handoffs between writing, encounter design, and presentation as parts of the same production problem. [Paweł Sasko, Commandments of Quest Design in CD PROJEKT RED, TGDF 2024](https://2024.tgdf.tw/en/speakers/2024-pawel-sasko) · [Conference recording](https://www.youtube.com/watch?v=HNAH-OVXWJ4)

## Techniques and practical checks

### Model quest progression as conditional transitions

Give quest facts stable identifiers and define what changes them. For each transition, specify prerequisites, the event that triggers it, and effects on objectives, journal, world state, and rewards. Represent facts that can happen out of order independently, then derive the current objective from their combination. Persist completion and irreversible effects together when possible.

**Check** by resolving an objective before accepting its quest, then by reloading immediately after its completion event. Verify that journal and world state agree and that the same reward can be granted only once.

### Rejoin branches while preserving consequences

Sketch distinct approaches and identify their merge points. Track the consequences promised by each branch, such as who trusts the player, what resource they spent, and what place they can visit. Carry those facts past the merge so later dialogue and gameplay can respond. Give each supported character build a route through mandatory gates.

**Check** by playing each approach from the same starting state. Verify that shared later scenes remain reachable and reflect each route's promised consequences. Test a character without optional abilities and a player who spends the expected resource elsewhere.

### Make the current intent recoverable

Give each objective a player-facing verb, target, and purpose. Update the journal from the same facts that drive quest progression. Provide a fallback cue when a guided route or character becomes unavailable, such as a journal recap or an alternate point of contact.

**Check** by resuming a mid-quest save without a recap from the tester. Ask what the player intends to do next and why. Repeat after leaving the region, skipping a conversation, and discovering an objective through exploration.

### Verify narrative logic and playable sequencing together

Make a small fixture for each prerequisite, branch, and failure policy. Walk the state graph with representative inventories and world conditions to find unreachable outcomes. Play through a blockout with placeholder dialogue and objectives before recording voices or finalizing cinematics.

**Check** for unreachable flags, mandatory NPCs becoming unavailable, and transitions that trigger twice. Play the actual quest with both immediate and delayed responses to prompts. Confirm that the playable route, character dialogue, and journal convey the same consequences.
