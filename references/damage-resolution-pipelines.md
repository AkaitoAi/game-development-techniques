# Damage resolution pipelines

## Problem and design reasoning

Fictional combat outcomes become inconsistent when contacts, armor, immunity, scoring, and reactions each mutate health independently. The released *Quake III Arena* implementation routes damage through one function. Its order is meaningful. Knockback precedes self-damage reduction, with a source comment explaining that this preserves rocket jumping. Armor absorption and health changes occur later, and the client accumulates feedback data for the end of the frame. This is direct evidence that gameplay intent can determine evaluation order. [id Software, Quake III damage implementation](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/g_combat.c)

Dan Reed's GDC 2017 talk explains why *Overwatch* used synchronized, modular Statescript graphs to define high-level weapon and ability state. It documents a larger ownership problem around gameplay logic. The staged resolver below is a portable recommendation, rather than a claim that Blizzard used this particular pipeline. [Blizzard, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities)

## Programming techniques and practical checks

### Carry an immutable request into one resolver

Represent the attempted effect with a source, instigator, target, activation ID, simulation tick, and relevant tags. Resolve a live target handle before applying it. Let the authority that owns target state commit health and related values, and return an explicit result such as rejected, absorbed, applied, or lethal.

**Check** send the same activation through two overlapping contact callbacks, then destroy and recycle the target before a delayed callback arrives. Verify the intended number of hits and that a stale handle never damages the replacement.

### Make modifier order a rule

Document eligibility, outgoing modifiers, target protection, shields, health, and secondary effects in the order the game's rules require. Specify rounding and minimum values at defined boundaries. Keep authored modifiers inspectable and log intermediate values for a selected request.

**Check** calculate fixtures by hand for immunity, shield overflow, a percentage reduction plus a flat reduction, and self-inflicted effects. Compare every intermediate stage with the logged result and test boundary values around zero.

### Commit the outcome before reactions run

Apply health, resource consumption, and the alive-to-dead transition as one simulation operation. Dispatch reactions after the result is committed. Queue reflected or chained effects with bounded depth and an explicit order, so a callback cannot leave the original operation half-applied.

**Check** apply two lethal requests in one tick and a reactive effect that removes its own source. Verify one death transition, correct credit, and a bounded sequence that ends in a valid state.

### Derive presentation from the resolved outcome

Give audio, particles, hit markers, and logs a result ID and the actual absorbed and applied amounts. Under prediction, define which feedback can appear immediately and how it is confirmed or corrected. Keep gameplay mutation out of presentation callbacks.

**Check** replay or reconcile the same request and verify one reward and the intended number of effects. Compare blocked, absorbed, and applied hits with audio muted and visual effects reduced.

For contact detection see [Melee hit detection](melee-hit-detection.md). For ongoing modifiers see [Status effects](status-effects.md).
