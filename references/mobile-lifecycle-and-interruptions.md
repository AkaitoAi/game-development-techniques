# Mobile lifecycle and interruptions

## Problem and design reasoning

On phones, the player can lose input or audio focus, have the game suspended, or return after the process has been reclaimed. Those events can arrive separately. Typical symptoms include music resuming on the lock screen, a held touch turning into an extra action, lost progress, and matches returning to stale state. In his GDC presentation on Vendetta Online, John Bergman identifies incoming-call backgrounding, preservation of network state, and process termination as distinct mobile port concerns. The practical inference is to design for both warm resume and cold recovery, then divide input, audio, rendering, and session ownership so each subsystem can suspend safely. [John Bergman, “Big Games in Small Places,” GDC 2012 slides](https://media.gdcvault.com/gdc2012/slides/Missing%20Presentations/Bergman_John_Big_Games_In.pdf)

## Shipped examples

- **Vendetta Online** ships on Android and iOS alongside desktop versions. Its persistent online universe makes backgrounding a client lifecycle event while the world remains active. Bergman's mobile port presentation explicitly calls out maintaining network state when an incoming call backgrounds the client. The slides support the design concern and give recommendations. They do not establish a specific reconnect protocol. [Vendetta Online platform and game overview](https://www.vendetta-online.com/h/welcome.html) · [GDC 2012 slides](https://media.gdcvault.com/gdc2012/slides/Missing%20Presentations/Bergman_John_Big_Games_In.pdf)
- **LIMBO on iOS** documents an audio interruption issue related to VoIP apps. For a mobile QA plan, this is evidence that a foreground game can have an audio problem caused by another app's session state, even when rendering and controls work. [Playdead's LIMBO iOS support notes](https://playdead.com/support/limbo/faqios.php)

## Techniques and practical checks

### Define independent suspend policies

Track foreground visibility, gameplay input focus, audio focus, and session connection independently. Pause local gameplay when control is lost, clear held actions, and pause or duck audio according to platform events. For shared sessions, show an appropriate return state while the server continues the match. Make lifecycle callbacks idempotent so repeated notifications leave the same final state. [Apple background execution sequence](https://developer.apple.com/documentation/uikit/about-the-background-execution-sequence)

**Check** an incoming call, notification shade, lock screen, audio focus loss, and normal app switch while touching the screen. Verify no phantom input or audio leak and confirm each interactive screen returns to a usable state.

### Save safe progress before suspension

Persist at gameplay boundaries such as completed objectives, menus, and committed rewards. On background entry, flush only crucial pending state within the allowed time and maintain the last committed checkpoint. Include a resume marker that identifies the save version and intended safe loading point. [Apple, preparing a UI for the background](https://developer.apple.com/documentation/uikit/preparing-your-ui-to-run-in-the-background)

**Check** return immediately from the app switcher, then repeat after process reclamation. Interrupt a save in a controlled test, restart, and verify checkpoint selection and any earned reward appear once.

### Rebuild transient resources on demand

Treat device and rendering resources as transient. Recreate invalid surfaces, focus or controller bindings, and audio devices on return. Keep ordinary resume on a fast path and fall back to loading only for resources that became invalid.

**Check** a short switch, a long switch with memory pressure, an orientation change, and a Bluetooth audio route change. Record resume-to-input timing and confirm there is no black frame, stuck loading state, or lost audio.

### Choose a clock for each system

Use simulation time for physics and pauseable timers, monotonic elapsed time for durations within a live session, and authoritative service time for online expirations when needed. Recompute state on resume from the relevant clock and avoid consuming a suspended interval as one simulation tick.

**Check** background for several minutes during a timed objective, a cooldown, and a live match. Verify the local simulation stays stable, the intended timers advance or pause, and the live game restores authoritative state.
