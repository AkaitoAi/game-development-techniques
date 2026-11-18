# UI empty and loading states

## Problem and design reasoning

An empty list can mean no owned items, a restrictive filter, an incomplete request, or a failed request. Displaying all four as a blank panel leaves the player unable to choose a next action. Maxis's *SimCity* UI presentation documents a shipped interface with a loading/status screen and a system supporting dynamic and network-backed content. David Candland's *Destiny* talk describes the challenge of serving new and experienced players and discusses time-gated interactions. These are shipped contexts for asynchronous UI, not evidence of a specific empty-state implementation. The state model below is a portable recommendation. [Yee Cheng Chin, How to Implement AAA Game UI, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Chin_YeeCheng_HowToImplement.pdf) · [David Candland, Tenacious Design and the Interface of Destiny, GDC 2016](https://gdcvault.com/play/1023460/Tenacious-Design-and-The-Interface)

See [UI error and recovery design](ui-error-and-recovery-design.md) for actionable failures and [UI list virtualization](ui-list-virtualization.md) for recycled content.

## Techniques and practical checks

### Give each absence a distinct state

Represent initial, loading, ready-with-content, ready-empty, filtered-empty, and failed as explicit view states. Define whether previously loaded content remains useful while a refresh runs.

**Check** supply zero records, a filter matching nothing, a delayed successful response, and a failed request. Ask a player what happened and what they can do next; compare that answer with the actual state.

### Describe progress honestly

Use a determinate progress bar only when a meaningful completed fraction is available. For indeterminate work, identify the operation and offer cancellation or a documented timeout path where appropriate.

**Check** slow or pause each loading stage. Verify the indicator keeps representing the current operation, avoids a false completion claim, and permits recovery after the supported timeout.

### Keep focus and content stable during refresh

Retain selection by logical item identity when content changes. Decide when a loading overlay owns input and which existing actions stay valid. Mark stale-but-usable data when acting on it can produce a different result.

**Check** refresh while using a controller, delete the selected item in the incoming dataset, and switch tabs before completion. Verify focus lands on a valid control and the old request cannot overwrite the newly selected view.

### Offer a relevant next action

For a true empty state, explain how content becomes available or provide a direct route to the relevant activity. For filtered emptiness, expose clearing the filter. For failure, preserve the player's context and offer the correct retry.

**Check** open the panel from a fresh profile, a populated profile with filters, and an offline session. Exercise the suggested action and return. Confirm the message, selected view, and content state update together.
