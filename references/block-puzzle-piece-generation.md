# Block puzzle piece generation

## Problem and design reasoning

**Problem.** In a Block Blast-style placement puzzle, the player places a tray of three pieces on an 8×8 board. Pieces cannot be rotated, and full rows and columns clear. The tray is the main difficulty control. Uniform random draws produce unwinnable trays early, trivial trays late, and streaks that players read as unfair. A selector that reacts to the board can shape pacing, but adaptation that players detect and cannot see explained reads as rigging.

**Developer reasoning.** Hungry Studio VP Jason Wang told MobileGamer that the studio runs more than 50 A/B tests a week, and that about 97% fail. He also said a Block AI Robot imitates real player interactions to test game scenarios and adapt to skill levels and playstyles. A January 2026 press release, quoted in The Star, reported more than 10,000 A/B tests in 2025 and an in-house player-behaviour simulator called Block AI. These sources document heavy experimentation and bot-based evaluation. They do not document the selector's rules. [MobileGamer interview](https://mobilegamer.biz/block-blast-maker-hungry-studio-a-b-tested-its-way-to-the-top-of-the-charts/) · [The Star (Inc./Tribune News Service), 28 Oct 2026](https://www.thestar.com.my/tech/tech-news/2026/10/28/is-block-blast-rigged-a-developer-decompiled-the-viral-game-to-find-out)

### Shipped precedents with documented rules

- **Tetris Guideline Random Generator.** The 7-bag deals one of each tetromino before reshuffling. This caps the gap between I pieces at 12 and S/Z runs at 4. [Hard Drop wiki](https://harddrop.com/wiki/Bag)
- **Tetris The Grand Master.** TGM redraws a piece that matches the four most recent pieces. TGM1 tries up to 4 times and later games up to 6, and about 3.5% of picks still repeat with 6 tries. The first piece is never S, Z, or O. [Hard Drop wiki](https://harddrop.com/wiki/TGM_randomizer)
- **Bastet.** Federico Poloni's deliberately hostile Tetris selector tries every piece in every legal position and scores the best result for each. Version 0.43 deals the worst piece 80% of the time and the next three 12%, 6%, and 2%. It never deals the best three. It shows what a hostile selector looks like. [Poloni's algorithm notes](https://pages.di.unipi.it/fpoloni/prog/bastetalgo.html)
- **Electronic Arts' dynamic difficulty research.** EA modeled player progression as a probabilistic graph and tuned difficulty to keep players engaged. It reported up to a 9% engagement improvement across several games, with no effect on monetization. [WWW 2017 paper](https://doi.org/10.1145/3041021.3054170)

### Reverse-engineered claims (unverified)

In October 2026, a third-party repository published symbol names from the iOS binary of Block Blast 7.5.3. It is not linked here because it redistributes the game. The names include a family of `selectBlock*` functions that share an `AlgoSelectInputArgs` input. The repository claims 77 named strategies and a classifier that rates players by ad value (`isHighValueForAdType:withDict:`) and by a maturity score.

Separately, The Star reported developer Ayomide Omolewa's decompilation claims. These include a "death algorithm" that deals an unplayable tray when three conditions hold:

- the player has just finished their recent rewarded ads,
- the next ad is worth at least 3.5 times the previous one,
- the player is 1,000 points past their high score.

The reported inputs also include country, phone model, and past ad revenue. Easier pieces reportedly appear when the player is well below their high score. Hungry Studio did not reply to the article.

Neither source is verified, and the two may not be independent. A symbol name does not show whether or how often a function runs. The table reads each name as a design hypothesis.

| Reported name | Inferred role (inference) | Portable technique below |
|---|---|---|
| `selectBlockRandomNoDie` | Random tray, filtered so that some placement order works | Solvability floor |
| `selectBlockEasyChoose` | Bias toward pieces that fit the current board | Bounded easing |
| `selectBlockFillPrefect` (sic) | Pieces that complete lines exactly | Reward moments |
| `selectBlockReviveReplace` | A new tray after a continue | Honest revive |
| `selectBlockEntropy3Medium`, `selectBlockIncreaseVariance` | Narrow or widen the variety of shapes in a tray | Variance control |
| `selectBlockHardOptimize`, `selectBlockHardLevel4` | Difficulty tiers | Board-state pacing |
| `selectBlockAvoidClearBoard`, `selectBlockStopRefreshScoreIOS` | Limit repeated board clears and score spikes | Board-state pacing |
| `selectBlockDealDeathState` | A tray with no legal placement | Cautions in the revive and segmentation sections |

## Techniques and practical checks

### Generate candidates, evaluate them, then apply a policy

Draw candidate trays from a weighted piece pool. Evaluate each tray against the current board with a solver. Then let a named policy choose a tray for the target difficulty.

- **Board model.** Store the 8×8 board as a 64-bit mask. Precompute each piece's mask at every anchor position. A placement is legal when `(board & piece) == 0`. After each placement, test the 8 row masks and 8 column masks for clears.
- **Solver.** Try all 6 placement orders, applying clears between placements. A piece may fit only after an earlier piece clears a line.
- **Scores.** Record whether the tray is placeable, the lines cleared, the fill ratio, fragmented holes, and how many piece families still fit somewhere.
- **Policies.** Keep each policy as a data-driven rule. Give all rules one shared input record, like the reported `selectBlock*` family, so each rule can be logged and A/B tested on its own.

**Check** log each dealt tray with its rule, candidate count, placeability, and scores. Replay a recorded seed and board, and confirm that the same rule and tray come back. Fuzz random boards and compare the solver's placeability verdict against brute force.

### Set a solvability floor and say when it lapses

Decide what share of trays must have at least one placement order in which all three pieces fit. A full board ending the run is an honest outcome. Building a tray with no legal placement is a separate design decision that players experience as rigging. The reported claims and the attention they drew (The Star cites more than 15 million views) show the trust cost.

**Check** count unplaceable trays by board fill ratio, and compare against a uniform random baseline over simulated games. Confirm that the floor is active in tutorials and first sessions, and that nothing builds an unplaceable tray on purpose.

### Control variance and droughts like a randomizer

Group shapes into families such as single cells, lines, L shapes, and squares. Adapt bag or history-and-redraw methods to those families. Cap repeats within a tray and across recent trays, and set a maximum drought for small pieces that rescue a crowded board. To control variety, measure the entropy of piece families over the last few trays and steer it toward a target band. The reported `Entropy3Medium` and `IncreaseVariance` names suggest a similar control (inference).

**Check** simulate 100,000 trays for each policy. Report the longest drought per family, repeat rates, and the number of distinct families per tray. Compare these against how often playtesters report unfair streaks.

### Pace with board-state targets rather than hostile pieces

Choose difficulty targets in board terms, such as fill ratio, fragmentation, and the largest empty rectangle.

- **Easing.** Ease off after crowded boards and repeated near misses.
- **Tightening.** Tighten when the board has stayed nearly empty for many trays. The reported `AvoidClearBoard` and `StopRefreshScore` names suggest limits on repeated full clears and score spikes (inference).
- **Means.** Raise difficulty by changing tray composition, for example more large pieces and fewer line-finishing pieces, rather than by dealing dead trays.
- **Limits.** Cap each change and add cooldowns, as described in [Difficulty and balancing](difficulty-and-balancing.md).

**Check** run bots of several skill levels: random legal placement, greedy line clearing, and a search over all three placement orders. Plot fill ratio and score rate over time for each. Confirm that every bot reaches its target run-length distribution without oscillating. Confirm that a strong bot's runs end because the board fills up, not because of one dead tray.

### Keep revive and continue offers honest

When a run ends, the continue offer (a rewarded ad or currency) must not depend on a death the selector caused on purpose. After the continue, deal a tray that can be placed. Derive it from the saved board and seed so that the order of ad SDK callbacks cannot change it. Save the restored board and the new tray before showing the offer, and keep the reward grant idempotent as described in [Mobile rewarded ads](mobile-rewarded-ads.md).

**Check** compare runs that decline every offer with runs that accept. Confirm that the distribution of why runs end before the offer is the same regardless of ad fill or ad value. Close the app during the ad and confirm that the revive tray is restored exactly once.

### Limit selector inputs to skill and experience signals

Feed the selector signals about the player's experience, such as recent failures, near misses, session count, and time since the last clear. Leave monetization signals out of the difficulty inputs, including ad eCPM, predicted ad value, past ad revenue, and purchase history. Also leave out country and device model unless they matter for performance. The ad-value, country, and device inputs in the unverified Block Blast claims are examples of what to keep out. Hidden difficulty tied to revenue costs player trust and draws attention from regulators who review manipulative design.

**Check** keep an allowlist of the selector's input fields and compare it against the input record in CI. Run the same board and seed with different ad-value, country, and device fields, and assert that the trays are identical.

### Test policies with bots before players

Run each policy across many fixed seeds with the bot tiers above. Measure:

- run length and score,
- why each run ends (no fit, or a continue was offered),
- how often each rule activates.

Then confirm with an A/B test on real players. Judge the results against the fairness measures above as well as retention. Hungry Studio's description of Block AI suggests the same order of work: simulation first, then live experiments.

**Check** pin seeds and bot results in CI. Flag any change that moves the mix of run endings or the rule activation rates past a set threshold.

## Related guides

Use [Puzzle design](puzzle-design.md) for rule teaching and solvability, [Deterministic random streams](deterministic-random-streams.md) for seeding trays so they can be replayed, [Procedural generation](procedural-generation.md) for checking that generated content is playable, [Difficulty communication](difficulty-communication.md) for explaining assists to players, and [Gameplay telemetry](gameplay-telemetry.md) for logging rule activations.
