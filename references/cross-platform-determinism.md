# Cross-platform determinism

## Problem and design reasoning

Two machines can process the same tick and input yet disagree on a small part of the simulation. Later rules can amplify that discrepancy into different game outcomes. Wube wanted PC and Nintendo Switch players in the same Factorio game, so it compared per-tick state CRCs between x86 and ARM and fixed incompatibilities found by the tests. A design inference is to define the deterministic state explicitly and put diagnostic boundaries around it. Effects that never feed simulation can evolve independently. [Wube, The journey to Nintendo Switch](https://www.factorio.com/blog/post/fff-370)

## Shipped examples

- **Factorio on Nintendo Switch (2022).** Wube identified platform-dependent behavior from code such as out-of-range floating-point-to-integer conversions. The team checked thousands of tests across CPU architectures to locate divergences. [Wube developer post](https://www.factorio.com/blog/post/fff-370)
- **Age of Empires IV (2021).** The development team's presentation describes preserving deterministic gameplay while evaluating it across machines with different core counts. Parallel work had to preserve the simulation's result. [GDC 2022, The MAW: Safely Multithreading the Deterministic Gameplay of Age of Empires IV](https://www.gdcvault.com/play/1027610/contactUs)

## Techniques and practical checks

### Declare the deterministic boundary

Write down which state is advanced from versioned initial data and tick-indexed commands. Keep wall-clock time, presentation choices, and local device capabilities outside that boundary. Take multiplayer commands through an agreed ordering before any gameplay mutation.

**Check** one input log in two builds with different display rates and frame pacing. Compare gameplay state at matching tick numbers and confirm that pausing presentation or dropping visual frames leaves the authoritative result intact.

### Specify numeric and random behavior

Choose representations according to supported ranges. Define rounding, saturation, and integer conversion explicitly where cross-platform agreement depends on them. Pin deterministic random algorithms, seeds, and state transitions. Separate unrelated random streams when optional work may change how often one is sampled.

**Check** minimum and maximum legal values, near-boundary conversions, zero-length geometry, and save or resume midway through a random sequence. Run fixtures across supported CPU, compiler, and build settings.

### Control ordering in parallel work

Ensure work results either commute or merge in a stable order. Give entities stable identities, stage mutations until a defined simulation phase, and specify a tie-breaker for operations with equal priority. Pay particular attention to floating-point reductions and conflicting writes.

**Check** the same stress scenario with one worker and multiple workers, varied task scheduling, and repeated runs. Include competing rule outcomes such as two units claiming the same tile in the same tick.

### Find the first divergent tick

Hash a canonical serialization of selected gameplay state using stable field and entity ordering. Keep build, data versions, and command logs with the result. Add subsystem hashes and targeted state dumps to distinguish the first difference from its later consequences.

**Check** inject a controlled difference into one test build to prove the harness detects it at the correct tick and names the right subsystem. Then replay cross-platform test fixtures and verify the matching build pair converges on identical hashes.
