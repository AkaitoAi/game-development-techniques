# Level encounter scripting

## Problem and design reasoning

An encounter script makes level-scale promises. It must start at an eligible point, communicate what changed, and reach a defined outcome even when the player backtracks, reloads, or reaches the objective by another route. The inferred design principle is to author the dramatic milestones and let reusable systems resolve moment-to-moment behavior. The boundary becomes particularly important where level designers own pacing and gameplay teams own actors, navigation, or combat.

## Shipped-game examples

- **Uncharted 4: A Thief's End.** Naughty Dog's Matthew Gallant described how larger spaces and more open-ended play led the team to explore systemic combat AI. The team initially leaned too heavily into systemic control and ultimately balanced authored content with systemic behavior. The talk frames that balance as a decision about how design and engineering share control. [Authored vs. Systemic: Finding a Balance for Combat AI in Uncharted 4, GDC 2017](https://www.gdcvault.com/play/1023949/Authored-vs-Systemic-Finding-a)
- **God of War Ragnarök.** Santa Monica Studio's Sam Sternklar described packaging script calls as events and processing them at a defined point in the frame. The documented reason was to let underlying systems change internally while keeping the scripting boundary stable. For encounter authors, an inferred benefit is that transition traces can show the events and state that produced a change. [God of War: Ragnarök’s Visual Scripting Solution, GDC 2023](https://media.gdcvault.com/gdc2023/Slides/God%2Bof%2BWar_Sternklar_Samuel%2B2023-03-31%2B07.08.17.pdf)

## Implementation and practical checks

Define an encounter lifecycle with explicit states such as dormant, active, resolving, and resolved. A volume crossing can request activation, but the transition should also read eligibility, ownership, and prior completion. Give each activation a stable encounter ID and each attempt a distinct attempt ID.

- **Check** Enter through each valid route, cross the boundary twice, then deliver the same activation event again. Verify that the intended attempt starts once and only its own participants and objectives are created.

Set the division between encounter milestones and actor decisions. For example, the encounter can resolve when a device is secured while independent actors choose actions using the gameplay system. State which completion conditions can substitute for a missing performer or scripted beat.

- **Check** Secure the device before the introductory beat, leave the arena during the beat, and remove an optional performer before completion. Confirm the exit and local objective agree with the current world state on every supported route.

Dispatch encounter events at a well-defined simulation boundary. Resolve simultaneous terminal events using explicit precedence and make durable effects such as unlocking an exit idempotent. Track each accepted or ignored event with the state before and after it.

- **Check** Deliver objective completion, player removal, and an actor timeout in the same tick. Repeat with varied arrival order where the design promises equal outcomes, and confirm the accepted terminal decision and emitted effects.

Treat checkpoints and streaming boundaries as inputs to the encounter lifecycle. Restore the authoritative outcome and participant state from a snapshot, then derive presentation from the restored state. Cleaning up an aborted attempt should release camera, music, gates, UI, and subscriptions owned by that attempt.

- **Check** Reload immediately before activation, in the middle of the encounter, and just after resolution. Unload and revisit if the game supports it. Confirm that the presentation matches each restored state and that any pending effects from the old attempt are ignored.
