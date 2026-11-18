# UI progress indicators for asynchronous work

## Problem and design reasoning

**Problem.** A progress bar reaches completion before the game is usable, stalls without explanation, or represents downloaded bytes while expensive activation work remains.

**Developer reasoning.** Nixxes explains that Ratchet & Clank: Rift Apart's PC dimension transitions require fast asset loading, decompression, and adaptive streaming. This documents multiple kinds of work, without specifying its progress UI algorithm. Adam Grodzki and Anshuman Prasad's GDC 2023 loading talk explicitly covers manifests and tracking download progress. The portable inference is to expose measured operation state and define what completion means before designing a bar. [Nixxes developer explanation](https://www.nixxes.com/project/ratchet-clank-rift-apart/) · [GDC 2023 loading talk](https://www.gdcvault.com/play/1029300/REVOLUTIONIZING-GAME-ASSET-LOADING-WITH)

## Techniques and practical checks

### Define the operation and completion contract

Give each load, save, install, craft, or export operation a stable identity and lifecycle. Define requested, working, finalizing, succeeded, failed, and cancelled states. Map success to the promised outcome. A load requires destination readiness, a save requires its durability contract, and an export requires a usable artifact. Reading bytes is one milestone. DirectStorage's completion notifications concern queued requests, illustrating why an application must define its wider readiness contract. [Microsoft completion semantics](https://learn.microsoft.com/en-us/gaming/gdk/docs/features/console/storage/directstorage/directstorage-overview)

**Check** delayed decompression, activation, and commit after reads finish. Confirm the indicator cannot report success until the operation's promised outcome holds.

### Represent known and unknown work honestly

Use a determinate bar when work has a meaningful denominator. Use an indeterminate state with a phase label when discovery or negotiation prevents that. Nested scopes can contribute weighted work to a parent, as illustrated by Epic's progress API. Calibrate weights from representative timings and keep download bytes separate from estimated total duration. Label estimates and update them only when enough observations exist. [Epic's scoped work model](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Core/FScopedSlowTask)

**Check** unequal file sizes, cache hits, newly discovered dependencies, and a near-empty operation. Confirm labels explain the current phase and that percentages describe the stated quantity.

### Separate animation from recorded work

Keep authoritative completed work independent of display interpolation. Smooth visual updates within measured bounds. When switching to a phase with an unknown total, show its name rather than inventing remaining completion. Display success or an error as a terminal state and expose accessible text in addition to movement and color.

**Check** very fast completion, bursty callbacks, zero progress for a long interval, and reduced motion settings. Confirm status remains readable and animation never implies successful work that has not happened.

### Handle interruption as part of the interface

Specify which phases permit cancellation, what partial data remains, and how retry resumes. Reject callbacks from replaced operation IDs. Keep the UI responsive while work runs and distinguish waiting for a service from locally active work. Link recovery messages to an action the player can actually take. Use [loading and transitions](loading-and-transitions.md) for world activation details.

**Check** cancellation during finalization, failure after partial completion, repeated retry, and leaving then reopening the screen. Confirm one terminal outcome per operation and no stale callback overwrites the current status.
