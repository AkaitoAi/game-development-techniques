# Game AI utility scoring

## Problem and design reasoning

**Problem.** Adding an action to a fixed priority list can create unintended interactions with existing actions. A companion might refuse to rest until a need crosses a hard threshold, then abruptly switch away from useful work. An NPC using raw distance, time, and need values can appear equally arbitrary if those values are incomparable.

**Documented reasoning.** ArenaNet's GDC presentation describes modular utility scoring as a way to author many different behavior sets for MMO agents without handcrafting a unique decision graph for each. EA's *The Sims 4* update notes document that a separate scoring path for follow-up socials had ignored negative relationship utility, causing repeated unwanted conversations. The fix applied eligibility rules to autonomous follow-ups while preserving player-directed socials. [ArenaNet and Intrinsic Algorithm, GDC 2015](https://www.gdcvault.com/play/1021848) · [EA autonomy update](https://www.ea.com/games/the-sims/the-sims-4/news/update-9-22-2026)

**Inference.** A reusable scorer is most valuable when many actions are simultaneously legal and their relative value changes with context. A small set of scripted sequences may be easier to author with explicit states.

## Shipped-game examples

- **Guild Wars 2: Heart of Thorns.** ArenaNet evaluates candidate decisions using considerations and chooses a high-scoring option for an agent's think cycle. The developer chapter details how authoring action inputs affects what agents choose. [Mike Lewis, Choosing Effective Utility-Based Considerations](https://www.gameaipro.com/GameAIPro3/GameAIPro3_Chapter13_Choosing_Effective_Utility-Based_Considerations.pdf)
- **The Sims 4.** EA documented a discrepancy between initial and follow-up social decisions. Its update skips relationship-scored autonomous follow-ups when the relationship utility is zero or negative. [EA autonomy update](https://www.ea.com/games/the-sims/the-sims-4/news/update-9-22-2026)

## Techniques and practical checks

### Filter for eligibility, then score

Build candidate action-target pairs, then filter using hard requirements such as availability, target validity, permissions, and cooldown. Score the surviving options and give the agent an explicit fallback when none qualify. Revalidate the selected action at execution time if world state can change between selection and commitment.

**Check** fixtures where a highly desirable action has a missing target or expired reservation. Verify that the agent chooses the next eligible action or the fallback and that the trace records the rejection reason.

### Normalize inputs and shape preferences

Map each input to a bounded domain before applying a response curve. Document curve meaning in designer terms, such as how urgently low energy competes with a nearby opportunity. A weighted average can combine independent preferences with calibrated weights. Use explicit gates for conditions that must veto an option. [Kevin Dill and Dave Mark, Improving AI Decision Modeling Through Utility Theory, GDC 2010](https://www.gdcvault.com/play/1012410/Improving-AI-Decision-Modeling-Through)

**Check** an input sweep from minimum to maximum, including exact range boundaries and missing values. Compare candidate scores and crossover points with the desired behavior. Repeat after adding a new consideration to catch accidental weighting changes.

### Stabilize choices over time

Set a minimum commitment or switch margin for sustained actions. Allow a higher-priority interruption on well-defined events. Break ties consistently when repeatability matters. If variety is desired, sample among similarly scored options at decision boundaries using a reproducible random stream for tests.

**Check** two near-equal actions under small periodic input changes, then introduce a legitimate interruption. Count switches, measure time to respond, and replay the same inputs to confirm tie handling.

### Expose the full decision trace

Record raw inputs, curve outputs, eligibility failures, and the selected action for sampled agents. Maintain fixtures for player orders and autonomous follow-ups separately when they have distinct rules. Budget candidate counts and update cadence on crowded scenes.

**Check** a baseline encounter and a dense stress case. Compare known expected choices with the trace, and measure candidate evaluations and decision latency. Follow a chosen action through execution to distinguish a scoring defect from a navigation or animation failure.
