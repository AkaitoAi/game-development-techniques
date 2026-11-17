# Puzzle design and solvability

## Problem and design reasoning

**Problem.** Players get stuck because they never learned a rule, or a level can reach a state with no valid solution and no clear recovery. Separate misunderstanding from reachability failures.

**Design reasoning.** Valve describes testing the basic act of passing through a portal before asking players to combine rules. An inference from Baba Is You is that games with editable rules need a precise resolution order so designers can test unexpected interactions. See the shipped examples below for sources.

Model a puzzle as states, legal actions, and a completion rule. Decide what the player should discover, then use playtests to see whether the information in the level supports that discovery.

### Shipped examples

**Portal (2007).** The team began playtesting with a room that simply asked players to walk through a portal. An early room with a small action set made it possible to observe how players understood a novel mechanic before asking them to combine it with other rules. [Valve developers’ postmortem](https://www.gamedeveloper.com/design/thinking-with-portals-creating-valve-s-new-ip) · [GDC 2008 talk](https://www.gdcvault.com/play/197/A-PORTAL-Post-Mortem-Integrating)

**Baba Is You (2019).** Players rearrange words to change the active rules of a block-pushing puzzle. Designer Arvi Teikari describes how the sentence system evolved during development, including its implementation in a game-creation tool with Lua. Rule edits themselves are puzzle moves, so interactions and update order are part of level design. [GDC 2020 technical talk](https://gdcvault.com/play/1026628/Reading-the-Rules-of-Baba) · [Games Now! developer talk](https://www.youtube.com/watch?v=9Y6TiY8RABs)

### Techniques and checks

**Prototype the discovery.** State the insight in one sentence. Build a small puzzle where the relevant component and the result of changing it can both be observed. Add a second puzzle that changes the arrangement while retaining the same rule.

**Check** blind attempts at both puzzles. Ask what outcome the player expects before the decisive move. A player who solves the first by accident and cannot transfer the rule may need clearer feedback.

**Validate the state space.** For small discrete puzzles, search legal moves from the actual starting state. Track visited states and keep a known solution as a fixture. Include orientation, active rules, inventory, and other state that changes legal moves. For continuous puzzles, record a successful path and probe nearby variants using the shipped movement and physics settings.

**Check** that the fixture wins, invalid actions leave the proper state, and each generated level is solvable within the validator's limits. Explore dead-end states and confirm the player can recover through undo, reset, or another move.

**Specify rule timing.** Choose when changes take effect within a move and how conflicting rules resolve. Keep win checks and visual state synchronized with the simulation. Record full prior state or reversible changes for undo.

**Check** simultaneous triggers, a rule change that alters who can move, and competing win and loss conditions. Undo and redo the critical move and compare the resulting state with replay from the beginning.

**Review alternate solutions and hints.** Keep alternate solutions that fit the intended learning goal. If a shortcut skips a prerequisite needed later, change the prerequisite or where it is taught. Offer hints that progress from pointing out a relevant object to describing a concrete action.

**Check** for bypasses using extreme movement, unexpected object orders, or starting after a checkpoint. Try each hint from the initial state and from a partially solved state. Verify that each hint describes an action or clue still available.
