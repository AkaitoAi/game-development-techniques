# Roguelike run design

## Problem and design reasoning

**Problem.** Runs diverge too early into victories or dead ends, or choices stop mattering after the first strong reward. Measure where builds narrow and how often recovery remains possible.

**Design reasoning.** Mega Crit documents using metrics and community feedback, and Supergiant describes powers designed to interact. An inferred tension is to allow strong combinations while keeping enough alternate paths for a run to remain worth playing. See the shipped examples below for sources.

## Shipped examples

- **Slay the Spire (2019).** Choices on the route and in rewards shape a deck that must handle later opponents. Mega Crit's Anthony Giovannetti describes using metrics and community feedback throughout Early Access to tune this loop. [Metrics Driven Design and Balance, GDC 2019](https://www.gdcvault.com/play/1025731/-Slay-the-Spire-Metrics)
- **Hades (2020).** Supergiant intentionally designed powers that interact and build on one another. Its developer talk covers how to represent power interactions and the tradeoffs that arise as the number of combinations grows. [All Together Now: Creating Multiplicative Power in Hades, Roguelike Celebration 2021](https://www.roguelike.club/speakers2021.html)

## Techniques and checks

### Pace the run in stages

Give the opening a reliable way to build toward a strategy, the middle a reason to adapt it, and the ending a test of what the player assembled. Budget opportunities to acquire power, repair a weak build, and spend resources before major gates. Let optional paths trade safety for upside.

**Check** ten fixed seeds with low, medium, and high risk routes. Record the first meaningful choice, the first viable build, resource levels at gates, and whether a player could adapt after a weak opening. Inspect seeds where every route imposes the same cost.

### Offer choices with different time horizons

Mix immediate utility, conditional upgrades, and flexible alternatives. Use explicit prerequisites for offers that require a prior item. Define the order of additive and multiplicative modifiers in one rule set, and consider caps where multiple independent multipliers accelerate damage, score, or resource generation.

**Check** a starter build, a synergy completed early, and a build that never finds its preferred reward. Confirm tooltips agree with resolution, investigate repeated forced picks, and measure the highest totals possible with overlapping effects.

### Separate run state from account state

Make a table of what resets, persists, and unlocks between runs. Give completed or failed runs persistent IDs and commit between-run rewards once. Add unlocks to validated offer pools so a new option fits the stage and prerequisites of the run.

**Check** defeat, retry, abandoning, and resuming during the results transition. Confirm benefits apply once and temporary modifiers clear. Re-run an early pool with a fresh and an experienced profile to check how unlocks change its distribution.

### Read balance by decision context

Log the seed and build version, offered options, selections, current build, and the stage when a run ends. Interpret pick and win rates against offer eligibility, player experience, and whether the chosen item tends to appear after a run is already strong.

**Check** compare a reward across early and late offers and across builds that can and cannot use it well. Reproduce suspected outliers with fixed inputs before changing rates or values. After tuning, check both variety and outcomes.
