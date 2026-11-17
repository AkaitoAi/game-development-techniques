# Gameplay ability prediction

## Problem and design reasoning

Waiting for a network round trip before an ability responds makes controls feel delayed. Showing an immediate result can leave the client displaying a spent charge, an effect on the wrong target, or a second cast cue when authority arrives. Ability prediction needs a way to identify a particular attempt and reconcile its state and presentation.

In shipped *Overwatch*, Blizzard's Statescript drives high-level state machines for weapons and abilities. Dan Reed describes automating prediction and replication of script behavior to help designers iterate on heroes while handling common networking concerns. This supports putting the reconciliation mechanism in shared infrastructure. [Dan Reed, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities)

Epic documents a separate framework example in which prediction keys associate a local activation and its provisional effects with the server response. Some chained activations need additional handling. The portable inference is that each predicted change needs a clear owner and an explicit rule for acceptance, rejection, or correction. [Epic Games, FPredictionKey](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Plugins/GameplayAbilities/FPredictionKey)

## Techniques and practical checks

- **Track each activation attempt.** Assign an identifier scoped to the controlling session and actor lifetime. Record the ability, the simulation tick or relevant input, and the provisional changes. The authoritative result should identify the same attempt. **Check** two quick casts of the same ability plus repeated and out-of-order responses in a local fault-injection fixture. Each result should resolve the intended attempt once.
- **Choose what can be provisional.** Predict responsive feedback and reversible local changes. Treat shared rewards, persistent inventory, and authoritative effects on other actors as committed only on confirmation. **Check** a local cast followed by an authoritative rejection because the target becomes invalid. Verify that the provisional cost and cooldown unwind and the target receives no committed effect.
- **Reconcile dependent changes.** Model predicted charges or costs as provisional deltas, or rebuild them from the last confirmed state and remaining pending attempts. Define what happens when a later action depends on an earlier predicted effect. **Check** an ability that grants a charge followed by an ability that consumes it. Reject the grant after both are pending and verify the dependency resolves consistently without overwriting unrelated confirmed changes.
- **Deduplicate or correct presentation.** Match cues by activation and cue identity. Promote a matching provisional cue and replace reversible feedback when the authoritative target or outcome differs. **Check** acceptance with matching state, acceptance with a corrected target, rejection, and actor despawn with a pending attempt. Record cue counts and confirm that no delayed callback changes a replacement actor.

See [Ability systems](ability-systems.md) for activation and effect rules, and [Resimulation-safe gameplay events](resimulation-safe-gameplay-events.md) for side effects that survive replay.
