# UI transition design

## Problem and design reasoning

A transition should explain a change in the interface while keeping the next action available at the intended time. Long or inconsistent transitions make frequent menus feel slow. Motion can also conflict with a game's visual language. Housemarque describes choosing little directional motion and flickering screens for *Returnal*, matching its industrial CRT aesthetic. That is a documented art and interaction choice. It does not establish a universal animation duration. [Housemarque, Returnal UX design](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

David Candland's *Destiny* presentation describes balancing approachability for newcomers with depth for experienced players, including time-gated interactions and the Director interface. A transferable design inference is to measure transitions along complete player tasks, including input and focus, while choosing a motion language that fits the game. The implementation contracts below are portable recommendations. [David Candland, Tenacious Design and The Interface of Destiny, GDC 2016](https://gdcvault.com/play/1023460/Tenacious-Design-and-The-Interface)

## Techniques and practical checks

### Make hierarchy changes legible

Define a small vocabulary for opening a nested view, returning to a parent, confirming an action, and displaying a modal. Use position, opacity, or another restrained cue to communicate the relationship between the outgoing and incoming state. Preserve enough continuity for players to locate their previous selection.

**Check** ask unbriefed players to open details, return, and open a modal. After each transition, ask which screen is active and where their selection went. Record mistaken back actions and time to the next intended action.

### Define when input ownership changes

Treat visual progress and interaction readiness as explicit state. Choose when the outgoing view stops accepting actions, when the incoming view receives focus, and what Back does while entering or leaving. If early input is queued, retain the intended action and destination rather than replaying a raw button press on a different screen.

**Check** press Confirm and Back at the start, midpoint, and end of each transition. Verify one navigation result, one active focus owner, and no click reaching a control hidden behind the modal.

### Make interruption preserve continuity

Start a replacement animation from the current displayed state. Retire completion callbacks from superseded transitions and define the settled state after repeated opening and closing. Reserve layout space for content that arrives asynchronously where a sudden shift would move the active target.

**Check** alternate open and close rapidly, delay a thumbnail load, change input device, and remove the selected item during a transition. Verify focus resolves to a valid control and old callbacks cannot reopen a retired view.

### Offer a reduced-motion path with equivalent information

Provide a restrained alternative for strong travel, zoom, or repeated effects. Keep the information and focus outcome consistent. Apple recommends cancellable UI motion and alternatives such as fades for reduced-motion preferences; adapt that guidance to the project's devices and presentation. [Apple motion guidance](https://developer.apple.com/design/human-interface-guidelines/motion)

**Check** complete the same menu task with full and reduced motion at the lowest supported frame rate. Compare task completion, focus restoration, responsiveness, and whether either mode requires waiting for cosmetic completion.

See [Tween systems](tween-systems.md) for interpolation ownership and [UI focus navigation programming](ui-focus-navigation-programming.md) for focus implementation.
