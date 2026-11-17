# Game user interface

## Problem and design reasoning

**Problem.** Players miss an urgent status cue during action, lose controller focus when the layout changes, or must leave gameplay to confirm a setting. Observe where attention and input focus are when the information is needed.

**Design reasoning.** Housemarque documents organizing Returnal's HUD around player attention in combat. Visceral's account illustrates a different constraint involving world integration. The inferred guide is to choose placement based on how quickly and reliably the cue must be seen. See the shipped examples below for sources.

Organize HUDs and menus around the decisions players must make. Choose each element's urgency, location, behavior under input, and lifetime.

## Shipped examples

**Dead Space (2008).** Visceral integrated interface elements into the game world, including suit-mounted status indicators and spatial displays. Producer Chuck Beaver describes using the interface to reinforce the character's equipment and setting. Lead interface designer Dino Ignacio documented the iterations behind that approach. [Beaver's production account](https://arstechnica.com/gaming/2008/05/guest-writer-dead-space-producer-chuck-beaver-on-story-ui/) · [Ignacio's GDC 2013 retrospective](https://www.youtube.com/watch?v=pXGWJRV1Zoc)

**Returnal (2021).** Housemarque organized HUD information around where players look during intense combat. It placed urgent cues near the reticle, used in-world elements where appropriate, and moved lower-priority information to the periphery. Its settings previewed controller remaps and other changes before the player returned to gameplay. [Johannes Koski, Housemarque UI/UX breakdown](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

Both approaches use fictional equipment as part of the visual design. Returnal's account also highlights the effect of scan time and where the player's attention is during a specific task. Determine which position gives each critical cue a reliable reading under normal camera movement.

## Techniques and practical checks

### Place information by decision urgency

For each HUD element, record what action it supports and how quickly the player needs to see it. Put time-critical cues close to the task they affect. Give persistent status a predictable location. Collapse or remove situational cues once they expire and apply explicit priorities when several cues compete.

**Check** by running a crowded encounter with several simultaneous events. Ask first-time players to describe the situation and choose an action without pausing. Log missed cues and conflicting alerts, then repeat with altered cue placement.

### Test world-space and screen-space presentations

Use world-space UI when the information belongs to a location or object the player can identify. Define behavior when the display is behind the camera, occluded, distant, or viewed at an angle. Provide a screen-space representation for important information when the world-space cue falls outside its effective reading range.

**Check** while orbiting the target, moving through occluders, and approaching from extreme angles. Repeat with wide and narrow aspect ratios, larger UI settings, and scene effects enabled.

### Give menus predictable focus and navigation

Store the intended default focus, ordering, and back destination for each screen. Distinguish moving focus from confirming an action. Restore focus to a useful location after a modal closes or the data backing a list changes. Use stable row IDs so focus follows the intended item through filtering and sorting where possible.

**Check** every required operation using only a directional input and confirm/back. Include disabled entries, empty lists, rapid changes, controller disconnects, and coming back from a child screen. Confirm focus is always visible and a clear exit remains available.

### Bind displays to gameplay state and preview settings

Render UI from a current state snapshot and use events for transitions or confirmations. Treat settings preview and commit as separate actions when a choice can make the interface difficult to use. Set a recovery policy for display changes and preserve the prior valid value.

**Check** entering a screen from a loaded save, receiving two updates in the same frame, and switching input while a confirmation is open. For a risky display setting, wait out confirmation and verify recovery. Exercise long localized strings, maximum text size, and right-to-left layout where supported.

### Rebuild directional navigation when the layout reflows

When scaling or a narrower screen changes a grid into a list, resolve directional neighbors from the new layout or author navigation for each responsive mode. Preserve the active element by ID when it still exists, then scroll it into view. Keep screen-reader order aligned with the visual flow. Forza Horizon 4 illustrates explicit directional behavior in a vertical menu. [Microsoft, Xbox Accessibility Guidelines on navigation](https://learn.microsoft.com/en-us/gaming/accessibility/xbox-accessibility-guidelines/112) · [Xbox Accessibility Guidelines on focus](https://learn.microsoft.com/en-us/xbox/accessibility/xbox-accessibility-guidelines/113)

**Check** by focusing a card in a multi-column layout, increasing text size until it reflows, and finishing the same task with directional controls alone. Then reverse the resize and repeat in a right-to-left locale where supported. Compare visual focus, scroll position, and narration at each step.
