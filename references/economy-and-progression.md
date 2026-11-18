# Economy and progression

## Problem and design reasoning

**Problem.** Resource balances grow without useful spending, reward routes converge on one dominant choice, or totals grow while players still miss key unlocks.

**Design reasoning.** CCP's reports document both flows and price outcomes. Supergiant describes changing Arcana and resource collection during Hades II Early Access. The design inference is to measure what players can choose and when, alongside the amount they earn.

## Shipped examples

**Diablo II (2000).** Blizzard's classic guide documents gold paid to gamble for unidentified items and Horadric Cube recipes that consume specific ingredients to produce another item. A currency sink and an ingredient conversion can both motivate repeated play while giving players different levels of control over the result. [Blizzard on gambling](https://classic.battle.net/Diablo2exp/basics/gambling.shtml) · [Blizzard's Cube recipes](https://classic.battle.net/Diablo2exp/items/cube.shtml)

**EVE Online (2003, live economy).** CCP's 2026 economic reporting tracks ISK faucets, mining, production, destruction and price indices. The Fanfest 2025 program includes developer presentations and panels on EVE economics. Track both flows and outcomes when player behavior can shift what a currency will buy. [CCP economic report, April 2026](https://www.eveonline.com/news/view/monthly-economic-report-april-2026) · [EVE Fanfest 2025 program](https://www.eveonline.com/news/view/eve-fanfest-2025-program-reveal-and-megablog)

**Hades II (2025).** Supergiant describes reworking Arcana effects and resource collection during Early Access. Those changes show why a progression test should track the builds and decisions players reach, alongside the quantities they earn. [Supergiant's Warsong Update](https://www.supergiantgames.com/blog/hades2-warsong-update/)

## Techniques and practical checks

### Account for sources, sinks and transfers

List each resource and every rule that creates, consumes, converts or transfers it. For each resource, reconcile ending balance with starting balance, grants, costs and net transfers. Define a conversion's input and output separately. For trading, account for both parties and any fees. Record a reason and stable transaction ID for each mutation where requests can be retried.

**Check** grant, purchase, trade, cancellation and duplicate-request fixtures. For a transfer, verify both balances and reconcile the change in overall supply to any fees. Exercise insufficient balance and a failed item delivery.

### Tune progression around player choices

Map milestones to new capabilities, routes or build options. For each milestone record prerequisites, earn rates, costs and the alternatives available at that point. If a required milestone depends on a random drop, consider a bounded fallback such as a guaranteed ingredient or targeted choice. Keep expected pacing and acceptable slow-tail pacing explicit.

**Check** fast, typical and low-yield playthroughs. Compare time to first meaningful choice, time between unlocks and the fraction of players able to reach a required milestone without relying on optional trade or a lucky drop.

### Simulate the routes players can repeat

Model different play patterns with a small spreadsheet or seeded simulator. Include time cost, resource conversion, caps, recurring maintenance and any compounding effects from unlocks. Compare route output against its intended role before changing rewards. For shared markets, also observe prices and trade volume by item class.

**Check** a new player, a specialist repeating the best known route and a veteran entering an update with a large stockpile. Look for cycles that create currency or power beyond the intended limits. Re-run the same scenarios after adjusting one rule.

### Preserve progress across balance changes

Track entitlement state independently of the current price table. Decide how an ongoing milestone or pending transaction crosses a configuration update. Document a recovery rule for failed conversions and ensure it can run again without duplicating a grant.

**Check** reload a save from just before and just after an unlock, then apply a revised cost and prerequisite configuration. Verify that every prior grant is accounted for once and that a path remains available through required content.

## Track rewards by lifetime

For each reward, specify whether it lasts for an encounter, a run, or the account. Keep progression grants and temporary modifiers in separate state, and describe what restarts and resets remove. **Check** award experience, unlock an ability, and pick up a temporary power-up before restarting an encounter and starting a new run. Verify that each reward expires or persists according to its lifetime. See [Skill trees and build diversity](skill-trees-and-build-diversity.md) and [Power-ups](power-ups.md).

## Validate reward requests at the mutation boundary

Validate requested reward identity against the current eligible progression state before indexing a reward table or granting anything. A disabled button or caller-side check can communicate eligibility, while the grant command enforces it for every caller. Commit the progression change and reward according to the project's persistence contract.

Exercise the existing grant command with a wrong reward/day identifier, negative and out-of-range identifiers, malformed saved progression, an expired eligibility window, and duplicate delivery. On rejection, compare currency, inventory, progression, timestamps, and emitted reward events with their initial values. On success, verify the exact grant and one progression transition. See [Gameplay invariants and validation](gameplay-invariants-and-validation.md), [Gameplay transaction boundaries](gameplay-transaction-boundaries.md), and [Game clock domains](game-clock-domains.md).
