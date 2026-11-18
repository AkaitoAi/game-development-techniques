# Animation layer composition

## Problem and design reasoning

Locomotion, aim, facial motion, breathing, and reactions can all affect a character at once. Without explicit composition rules, a later system can overwrite a pose another action needs. In shipped *Uncharted: Drake's Fortune*, Naughty Dog organized independent actions into animation layers, each selecting and transitioning states before contributing to a final pose. The GDC slides illustrate additive tired motion reused over different base gaits. [Christian Gyrling, Creating a Character in Uncharted, GDC 2008](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf)

For *Gears of War 4*, Microsoft developed inertialization to retain transition continuity while reducing the cost of evaluating both old and new states. That is evidence that transition policy also affects runtime evaluation cost. The contracts below are transferable recommendations rather than claims about either game's complete implementation. [David Bollo, Inertialization, GDC 2018](https://www.gdcvault.com/play/1025165/Inertialization-High-Performance-Animation-Transitions)

## Techniques and practical checks

- **Give each layer a defined contribution.** Specify its joints, blend mode, reference pose, priority, and weight owner. Use an appropriate reference when applying additive offsets so the same correction has a consistent meaning. **Check** the base pose alone, each individual layer, and all supported combinations. Inspect spine, shoulders, wrists, and root for accumulated offsets.
- **Make conflicts explicit.** Define whether a full-body action suppresses aim, recoil, locomotion, or secondary motion, and which inputs can interrupt it. Restore the previously valid contributions through a defined transition. **Check** aiming while moving, entering an interaction during recoil, and interruption during a full-body reaction. Verify there is one accepted owner for conflicting joints.
- **Choose evaluation order by dependency.** Evaluate the base, authored overrides and additives, then dependent corrections such as final contact solving in the order the feature requires. Keep authoritative action timing in its owning gameplay system. **Check** the same transition with exaggerated layer weights and final corrections disabled in a debug view. Identify which stage introduces any visible discontinuity.
- **Budget transitions and inactive layers.** Measure the cost of overlapping source states and remove work only when the layer's state and future reactivation remain correct. **Check** rapid alternating actions and low-priority characters changing animation tiers. Record evaluation time, pose continuity, event counts, and reactivation behavior.

See [Animation state machines](animation-state-machines.md) for transition eligibility and [Animation job pipelines](game-animation-job-pipelines.md) for parallel evaluation.
