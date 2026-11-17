# Gameplay hot reload

## Problem and design reasoning

Restarting the game after each edit makes it expensive to tune behavior that takes minutes to reach. Reloading into a running session also creates a new problem: old callbacks, in-flight work, and incompatible state can outlive the implementation that created them. Define which changes can safely go live and where the runtime can commit them.

**God of War Ragnarök.** Santa Monica Studio replaced running scripts with updated versions to shorten iteration. Sam Sternklar explains that scripts run to completion at a defined point in the frame, so requests are queued until just before the script phase. The safe point follows from the execution model. [Sam Sternklar, God of War Ragnarök's Visual Scripting Solution, GDC 2023 slides](https://media.gdcvault.com/gdc2023/Slides/God%2Bof%2BWar_Sternklar_Samuel%2B2023-03-31%2B07.08.17.pdf)

**Alan Wake 2.** Remedy describes changing its scripting technology to make live editing and rapid prototyping more accessible to game teams. The broader Northlight tooling also supports applying entity edits to a running process. A transferable inference is to choose boundaries around the edits designers repeat most, such as behavior definitions and encounter parameters. [Remedy annual report 2023, Northlight and Alan Wake 2](https://investors.remedygames.com/app/uploads/2024/03/remedy-annual-report-2023.pdf) · [Remedy, Real-Time World Editing Technology in Northlight, GDC](https://www.gdcvault.com/play/1034236)

## Techniques and practical checks

- **Validate a candidate before activation.** Compile it aside, resolve dependencies, and compare the interface and state schema against the active revision. Keep the active revision until the candidate can replace it. **Check** edit a gameplay rule to refer to a missing asset and submit it. Verify the last good revision remains active and the tooling points to the invalid reference.
- **Commit at a safe point.** Queue requests and apply them between simulation steps or at a known phase boundary. Drain work that can still reference the old implementation, or tag it with a generation and reject stale callbacks. **Check** request reload while a timer expires and a background job completes. Confirm the transition leaves one listener per subscription and never executes old work against new state.
- **Choose a state policy by edit class.** Preserve state across behavior-only edits. Migrate explicitly when compatible state fields change. Restart the affected scope when the schema cannot be mapped safely. **Check** shorten an active cooldown, add a state field, and remove a state that an entity currently occupies. Verify the documented outcomes and confirm durable progression was untouched.
- **Compare with a clean start.** Reuse a small snapshot or replayable scenario to distinguish reload artifacts from intended changes. Record the active revision and reload result in diagnostics. **Check** apply the same valid update repeatedly in an encounter, then start a fresh test session with that revision. Compare expected gameplay outcomes and watch for duplicate subscriptions, memory growth, or retained resources.
