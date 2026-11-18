# UI error and recovery design

## Problem and design reasoning

An error message can name a failure while leaving the player unsure whether progress was saved, an action completed, or retrying is safe. The shipped *Sea of Thieves* support catalog distinguishes account, connectivity, version, and service failures with named codes and recovery guidance. This establishes the value of distinguishing causes and next actions. It does not prove how Rare implements every in-game error view. [Rare, Beard Errors](https://support.seaofthieves.com/articles/115005299167-beard-errors)

Anran Li and Hyungjin Shin describe replacing a patchwork of interfaces with a unified UI system for *2XKO*, while supporting several platforms and ongoing development. Their GDC presentation supplies a production rationale for shared interface behavior. The recovery contracts below are portable recommendations derived from these needs. They focus on what happens after an operation fails, complementing navigation and general accessibility guidance. [Riot Games, Lessons from Building UI/UX in 2XKO, GDC 2026](https://www.gdcvault.com/play/1035668/Lessons-from-Building-UI-UX)

## Techniques and practical checks

### Explain the state and the next useful action

Translate the operation result into a short explanation of what failed, what is known about completion, and the action the player can take. Separate a temporary service problem from a request the player can correct. Keep a stable support code available without making the code carry the whole explanation. When completion is uncertain, reconcile the operation before offering another potentially duplicating submission.

**Check** inject a timeout before sending, after sending, and after server commitment. Ask an unbriefed player what happened and what Retry will do. Verify the displayed outcome agrees with the confirmed operation state and duplicate submissions follow the intended policy.

### Preserve work across recovery

Retain player choices such as a loadout or search filter while the operation retries. Define which state can be retained safely and whether the old choice is still valid after refreshed data arrives. Offer a valid exit path when the operation remains unavailable.

**Check** fail after the player configures a multi-step action, then recover with changed availability. Verify retained selections, explain invalid choices, and confirm Back returns to a usable view without losing unrelated progress.

### Give recovery views explicit ownership

Choose whether an error is a local inline state, notification, or modal based on which actions are blocked. Route focus to a meaningful recovery control and restore it when the error closes. Replace stale error views when the operation generation changes, including a retry started from another screen.

**Check** trigger two failures close together, dismiss one, change input device, and complete the older request late. Verify one actionable state for the current operation and that delayed results cannot reopen an obsolete modal.

### Test recovery as part of the player task

Use a failure matrix with the requested action, known outcome, preserved state, and recovery path. Include localization and assistive output in the same scenarios. Record recovery completion and repeated attempts as well as whether the dialog appeared.

**Check** complete the normal task with an unavailable dependency, expired eligibility, and a long translated message on the smallest supported layout. Verify the player can either recover or exit with a clear understanding of retained state.

See [UI transition design](ui-transition-design.md) for view changes and [Gameplay command validation](gameplay-command-validation.md) for enforcing valid mutations.
