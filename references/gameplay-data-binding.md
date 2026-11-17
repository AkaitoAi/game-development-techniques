# Gameplay data binding

## Problem and design reasoning

Widgets that each read live gameplay objects can disagree within a frame or keep observing an object after a player changes character. Conversely, a UI built entirely from transient change events can miss initial values when a panel opens late. Put a coherent read model between gameplay and presentation so both lifecycle cases have an explicit contract.

Maxis's GDC talk describes the web-technology UI shipped in *SimCity* (2013). Riot's *2XKO* GDC presentation describes how reusable widgets and encapsulated layers let teams work across different areas of the UI using consistent patterns. These talks document presentation architecture. Using an immutable per-screen snapshot and separating user intents from observed state are recommendations for applying that separation to gameplay binding. [Maxis, How to Implement AAA Game UI in HTML and JavaScript, GDC](https://www.gdcvault.com/play/1022054/) · [Riot Games, Lessons from Building UI/UX in 2XKO, GDC 2026](https://media.gdcvault.com/gdc2026/Slides/Anran_Li_Hyungjin_Shin_Lessons_from_Building_UI_UX_in_2XKO.pdf)

## Techniques and practical checks

### Derive display values from one committed state

Identify the authoritative owner and derive the values a screen needs after a simulation step or transaction. Publish a versioned snapshot with fields such as current health, maximum health, and whether the player's action is available. Keep formatted labels and interpolation in the presentation layer.

**Check** change current and maximum health together while issuing a heal. Record each displayed version and verify labels, gauges, and affordances use the same committed values.

### Read initial state and observe later changes

Expose an initial snapshot and subsequent changes as one subscription contract, or use version numbers to detect changes during initialization. Give each binding an owner such as a screen instance or local-player slot. Remove subscriptions when the owner closes or switches subjects.

**Check** open a panel while an update arrives, switch the observed player, and reopen the panel repeatedly. Verify the first render is current and the old player delivers zero callbacks after the switch.

### Send intents back through gameplay

Send an equip, interact, or purchase intent to the gameplay owner. Show pending, accepted, and rejected states where an operation is asynchronous. Reconcile against the latest committed snapshot if a response arrives after the panel closes.

**Check** quickly submit two actions, reject the first, then close and reopen the screen before the second completes. Compare the final screen with authoritative gameplay state and verify that neither response is assigned to the wrong action.

### Bound update work

Apply the latest committed state once per presentation update when intermediate values have no gameplay meaning. Preserve ordered notifications for discrete outcomes the player needs to see. Skip repainting unchanged values, and identify repeated list rows by item identity.

**Check** run a burst of inventory and combat updates with a large panel open. Measure update cost, check that every distinct outcome is represented, and verify scrolling or selection survives row reordering.
