# UI feedback and confirmation

## Problem and design reasoning

A player can press the right button yet remain unsure whether the game accepted the action, is still processing it, or committed the outcome. Feedback needs to communicate those different states at a useful time. In *Returnal*, Housemarque placed critical information near the reticle and iterated animation timing, color, and hierarchy to support intense combat. Its scanning interaction combines visuals, sound, and haptics. These are documented choices for keeping information available near the action. [Johannes Koski, Returnal UX design](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

Derek Sakamoto's GDC talk describes the vision and production process behind *Hearthstone*'s immersive interface. It provides shipped-game context for treating the interface as part of the game's experience. The acceptance states, confirmation policy, and test procedures below are portable recommendations, and are not claims about Blizzard's internal implementation. This guide concerns successful or pending actions. Use the error guide when an operation fails. [Derek Sakamoto, Hearthstone: How to Create an Immersive User Interface, GDC 2015](https://www.gdcvault.com/play/1022036/contactUs)

## Techniques and practical checks

### Separate acknowledgment from committed success

Give an input a prompt acknowledgment when useful, such as a pressed state or an eligible target highlight. Display pending work when the outcome takes longer. Reserve the success cue for the operation's confirmed result. Associate each cue with the current action instance so a delayed reply cannot complete an unrelated screen.

**Check** run an immediate action, a delayed action, and a cancelled action from the normal UI entry point. Ask a first-time player what state each is in. Verify that every success cue corresponds to an actual committed result and that pending feedback ends.

### Reserve confirmations for meaningful commitments

Show a confirmation where an uncommon choice has a consequence the player should review, such as spending a scarce resource or discarding a long configuration. State the object, consequence, and action clearly. For low-cost reversible actions, a clear outcome and an undo opportunity may serve the task better. Choose this policy according to the game's rules.

**Check** present a costly commitment and several routine actions using the intended input devices. Observe accidental activation, comprehension of the consequence, and repeated dismissal. Verify cancellation preserves the exact prior selection and resource state.

### Give feedback a consistent vocabulary

Use compatible visual, audio, and tactile cues for accepted, pending, completed, and unavailable states. Match intensity to significance and keep ordinary repeated actions from covering important information. Provide an accessible visual or text representation for an essential cue that otherwise depends on sound or vibration.

**Check** perform several actions in quick succession with sound muted, vibration disabled, and reduced motion enabled. Ask players to identify which action completed. Check that overlapping feedback remains associated with the correct target.

### Keep repeated input and presentation synchronized

Specify whether another press queues, replaces, repeats, or is ignored while work is pending. Derive availability from gameplay state and update the view when that state changes elsewhere. Preserve useful feedback even when a player skips its decorative animation.

**Check** press twice before the first completion, switch views, and return after an external state change. Confirm the intended number of commands, correct final availability, and no replay of an obsolete success animation.

See [UI error and recovery design](ui-error-and-recovery-design.md) for failures and [Gameplay command validation](gameplay-command-validation.md) for enforcing eligibility at mutation.
