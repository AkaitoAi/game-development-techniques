# Difficulty and balancing

## Problem and design reasoning

**Problem.** Challenge spikes have unclear causes, or a single difficulty knob changes timing, damage, and recovery together so the team cannot determine what helped.

**Design reasoning.** Valve documents pacing breaks based on survivor intensity. Supergiant documents opt-in damage resistance in Hades. A design inference is to distinguish encounter tempo from punishment and test each demand against the player experience it is meant to change.

Identify the skill each challenge is meant to exercise and how much failure costs. Adjust that challenge with parameters players can understand and the team can test.

### Shipped examples

**Left 4 Dead (2008).** Valve used a survivor intensity estimate to pace the threat population. When intensity reached a peak, the system sustained pressure briefly, let the current engagement fade, then provided a period of lower pressure. Adaptive pacing can vary the rhythm of play while retaining authored difficulty settings. [Mike Booth, AIIDE 2009](https://steamcdn-a.akamaihd.net/apps/valve/2009/ai_systems_of_l4d_mike_booth.pdf)

**Hades (2020).** Supergiant gives the player a God Mode option that increases damage resistance and adds further resistance after deaths. This changes how much punishment a run can absorb while leaving the player in control of whether the mode is active. [Supergiant developer FAQ](https://www.supergiantgames.com/blog/hades-faq/)

### Techniques and checks

**Separate demands in the tuning data.** Give encounter designers explicit controls for response time, simultaneous threats, resource pressure, incoming damage, and recovery time. Prefer editing one demand at a time when diagnosing a difficulty spike. Record how changes combine across difficulty presets.

**Check** the same encounter with identical inputs and seed across presets, then test with several player skill levels. Record failure reasons and recovery opportunities. Confirm that each preset changes the intended demand and preserves necessary cues.

**Measure the curve and the cost of failure.** Track attempts, completions, time to retry, and the amount of content replayed after failure. Compare adjacent stages and inspect common failure locations. Include players who leave or skip a challenge in the analysis.

**Check** a first-time run, a returning player's run, and a practiced run. Find any abrupt changes in repeated attempts or replay time. Observe whether players recognize the mistake and can make a different choice on the next attempt.

**Bound adaptive changes.** Select a small set of signals tied to the target experience, such as repeated failures or sustained pressure. Cap the adjustment, use thresholds or cooldowns to limit oscillation, and apply changes at safe points. For a player-selectable assist, specify what it changes and preserve the setting across reloads where appropriate.

**Check** alternating success and failure, the lowest and highest settings, and a save reload during adaptation. Confirm that intentional experimentation or pausing does not advance the adjustment by mistake.

**Test strategy diversity.** Pick a baseline strategy plus a few alternatives and compare their costs, risks, and outcomes in situations intended to favor different approaches. Include edge combinations of upgrades and consumables so balancing reflects how people actually build around mechanics.

**Check** whether one strategy wins across every target scenario under comparable execution. If it does, identify the underlying advantage before tuning it. Retest the broader set after changing a shared parameter.
