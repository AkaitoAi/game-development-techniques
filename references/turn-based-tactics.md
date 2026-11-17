# Turn-based tactics

## Problem and design reasoning

**Problem.** Players understand individual abilities but cannot predict a chain of effects, or previews disagree with resolution. Record the first unexpected rule and whether it was visible before commitment.

**Design reasoning.** Subset's telegraphed attacks in Into the Breach make much of the upcoming board state visible. The inferred design trade-off is that clearer previews can move the challenge toward prioritizing outcomes and sequencing decisions. See the shipped examples below for sources.

## Shipped examples

**XCOM: Enemy Unknown (2012).** Firaxis describes iterating on the connection between the campaign strategy layer and tactical combat deep into production. Its AI presentation discusses movement and ability selection for different alien types. Tactical actions can carry meaning beyond the immediate board, and distinct unit rules should produce recognizable opponent behavior. [Firaxis postmortem](https://www.gamedeveloper.com/design/classic-postmortem-i-xcom-enemy-unknown-i-which-turns-5-today) · [Firaxis AI postmortem, GDC 2013](https://gdcvault.com/play/1018223/AI-Postmortems-Assassin-s-Creed)

**Into the Breach (2018).** Subset Games telegraphs enemy attacks before resolution. The design postmortem traces how the team arrived at its tactics format. With attacks shown in advance, a player can evaluate outcomes that depend on the order of actions and changes to the board. [Subset Games, game overview](https://subsetgames.com/itb.html) · [Matthew Davis, GDC 2019](https://media.gdcvault.com/gdc2019/presentations/Into%20the%20Breach%20Postmortem%20Final.pdf)

**Phoenix Point (2019).** Snapshot Games combines a strategic Geoscape with tactical encounters where disabling a creature's body part can affect its capabilities. At EGX Rezzed, co-founders Julian Gollop and David Kaye discussed the project in development. Its shipped rules make location-specific conditions relevant to board decisions. [Snapshot Games, shipped features](https://phoenixpoint.info/) · [Developer conversation at EGX Rezzed 2018](https://www.youtube.com/watch?v=JksZBE17Y6o)

## Techniques and practical checks

### Make the action economy testable

Define the cost and eligibility of each command, the order in which a turn grants or refreshes resources, and which effects can alter those rules. List whether undo is allowed before a decision reveals new information. Resolve eligibility, reservation, and spending at explicit points.

**Check** issue commands at zero resources, exactly at cost, and after a mid-turn refund. Try a command whose target becomes invalid. Confirm the remaining resources and available actions after rejection, cancellation, and undo.

### Derive previews from game resolution

Use the same query or rules implementation for previews and committed actions. Show deterministic positional effects and ranges or odds for random outcomes. Rebuild the preview when a relevant board state changes, and distinguish where uncertainty comes from.

**Check** compare preview and execution for blocked movement, displacement into an occupied tile, an effect chained to a second unit, and a target with an expiring status. For random effects, check that all sampled outcomes are possible according to the displayed rules.

### Give reactions a stable order

Use explicit phases and a consistent order for simultaneous reactions. Include a tie-breaker that remains stable after saving and loading. Set a termination rule for nested reactions, then present the cause of each meaningful interruption to the player.

**Check** create simultaneous triggers, an interrupted move, and a reaction that removes another pending actor. Replay the same commands from a saved state and compare the event sequence and final positions.

### Measure decisions in representative scenarios

Vary objectives, unit budgets, and encounter length to test whether the same strategy dominates. Compare resource spend, time per turn, and objective completion. Track how tactical outcomes carry into any persistent campaign layer.

**Check** play the same board with two plausible plans, including a mission with a secondary objective. Verify that the expected tradeoff survives variation in initial positions and that a setback produces the intended campaign consequences.
