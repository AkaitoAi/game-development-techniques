# UI notification priority

## Problem and design reasoning

A reward toast, connection warning, tutorial, and immediate threat can compete for the same space. Rendering all of them immediately can make the most consequential message difficult to recognize. In *Returnal*, Housemarque describes keeping critical combat information close to the reticle and combining multiple warning channels near death. This documents an attention priority based on gameplay need. [Johannes Koski, Returnal UX design](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

David Candland's *Destiny* GDC presentation describes balancing an approachable interface with the depth experienced players need. That is relevant context for information volume, but the session summary does not establish Destiny's exact notification scheduler. The priority, expiry, and queue policies below are portable recommendations. This guide concerns which message gets attention when several requests compete. [David Candland, Tenacious Design and The Interface of Destiny, GDC 2016](https://gdcvault.com/play/1023460/Tenacious-Design-and-The-Interface)

## Techniques and practical checks

### Classify by urgency and useful lifetime

Give each message an owner, category, priority, earliest display time, expiry condition, and recovery destination. Reserve immediate attention for information that changes the player's next action. Keep a recoverable history for useful messages displaced by more urgent ones.

**Check** request a reward, objective update, and urgent gameplay warning on the same frame. Verify the intended order, that the warning stays readable, and that the reward and objective remain retrievable after the interruption.

### Choose replacement and coalescing rules

Define whether repeated messages accumulate, replace an existing value, increment a count, or wait. Group only messages that retain the same meaning when merged. Bound queue capacity and record which policy handles an overflow.

**Check** generate a burst above the queue limit, including several updates to one objective and several distinct rewards. Confirm that the displayed totals and history match the committed results and that the queue returns to baseline.

### Revalidate a message before display

When a queued message reaches the front, check that its target, context, and recommended action remain valid. A cancelled quest should retire its old prompt. Do not turn an expired urgent instruction into a misleading late cue.

**Check** queue a location-specific tutorial, leave the location, complete the objective elsewhere, and then clear the queue. Verify obsolete instructions stay suppressed while any still-relevant outcome remains available.

### Keep attention and input ownership explicit

Separate a passive toast from a modal that takes control. Specify which messages can interrupt aiming, menus, or cinematics and how focus returns. Keep text speed, narration, and reduced-motion settings compatible with expiry rules.

**Check** trigger each category during combat, a modal menu, and a cutscene with a controller and screen narration enabled where supported. Verify focus returns to the prior valid control and players can identify the urgent message before its action window closes.

See [UI information hierarchy](ui-information-hierarchy.md) for the visual layout and [UI feedback and confirmation](ui-feedback-and-confirmation.md) for the states of an individual action.
