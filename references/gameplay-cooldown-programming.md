# Gameplay cooldown programming

## Problem and design reasoning

A cooldown is an eligibility rule whose state may be consumed by input, prediction, charges, modifiers, and UI. A callback that merely enables a button cannot enforce the rule for every caller. The released *Quake III Arena* weapon update decrements weapon time before checking whether firing may proceed, applies state and ammunition rules, and adds the selected firing delay. Its mission-pack modifiers also alter that delay. This makes timing part of the simulation's action resolution. [id Software, PM_Weapon](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/bg_pmove.c)

Dan Reed's *Overwatch* talk describes Statescript as the high-level logic for weapons and abilities and explains automated prediction and replication of that behavior. It establishes why such rules need an explicit networked execution model. The charge ledger and modifier policies below are portable recommendations, rather than a claim about Blizzard's specific cooldown representation. [Dan Reed, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities)

## Techniques and practical checks

### Check eligibility at action commitment

Keep cooldown state with the action's authoritative owner. Define whether cooldown begins at accepted activation, impact, completion, or another explicit commit point. Read the chosen simulation clock and consume eligibility alongside the action's cost. Derive UI availability from that state.

**Check** submit two activations in the same tick, interrupt just before and after commit, and attempt activation through a caller bypassing the UI. Verify one accepted action, the specified cost, and the correct deadline.

### Define charge recovery as a state transition

Specify whether charges recover sequentially or independently, which interval begins after a spent charge, and what happens at maximum capacity. Store the data required to reproduce the next recovery rather than treating a visual countdown as the owner.

**Check** spend two charges near one recovery boundary, then advance across several deadlines in a hitch. Compare recovered count and next deadline with the selected policy, including maximum-capacity behavior.

### Specify modifier and reset behavior

Choose whether a duration modifier changes only future cooldowns, preserves progress fraction, or adjusts the current deadline. Define resets and reductions in one place and make their event order explicit. Keep remaining duration bounded according to the action's rule.

**Check** add and remove a speed modifier halfway through recovery, apply a reset and reduction on the same tick, and change loadout. Verify the documented remaining duration and that removed abilities cannot receive an obsolete recovery callback.

### Reconcile presentation with authoritative state

When predicting activation, retain the action identity needed to accept or reject the predicted cooldown. Present remaining time using the agreed clock conversion and clamp display values at the boundary. Persist cooldowns only when the game promises survival across session changes.

**Check** delay acceptance, reject an activation, replay the accepted tick, and respawn during recovery. Verify no duplicate charge consumption, that UI and authoritative eligibility agree at expiry, and that respawn follows the intended retention policy.

See [Game clock domains](game-clock-domains.md) for pause and deadline boundaries, [Gameplay timer schedulers](gameplay-timer-schedulers.md) for dispatch mechanics, and [Gameplay ability prediction](gameplay-ability-prediction.md) for reconciliation.
