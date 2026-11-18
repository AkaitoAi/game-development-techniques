# Motion warping and alignment

## Problem and design reasoning

An interaction clip authored for one distance can miss a differently placed object or demand many near-identical assets. For shipped *Gears of War 4*, The Coalition generalized spatial adaptation around warp points, addressing animation coverage while preserving animator control. Steven Dickinson describes the content cost and limits of combining many similar clips in his GDC 2017 talk. [Motion Warping in Gears of War 4](https://www.gdcvault.com/play/1024219/Creating-Consistent-Game-Characters)

Naughty Dog's shipped *Uncharted: Drake's Fortune* used character and reference locators for object-relative action playback, including cover and vehicle actions. This provides an earlier example of expressing an action relative to an environmental reference. The recommendations below infer implementation contracts from these documented approaches. [Christian Gyrling, Creating a Character in Uncharted, GDC 2008](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf)

## Techniques and practical checks

- **Define the alignment contract.** Store the target frame, the authored reference frame, the adjustment window, and permitted translation and rotation ranges. Reject targets outside the supported envelope or choose another action. **Check** the smallest and largest accepted gaps, a rotated target, and a target just beyond the limit. Verify that rejection occurs before the actor commits.
- **Assign movement ownership.** Decide how animation displacement reaches the controller and collision system. Solve the intended adjustment, constrain actual movement against the world, and route an obstructed result to an explicit interruption or recovery. **Check** a moving obstacle entering halfway through the action and a low ceiling at the destination. Confirm the controller stays outside geometry and the action ends in a legal state.
- **Freeze or track the target deliberately.** Choose whether an action commits to its initial target frame or follows later target motion. Keep that decision consistent across root movement and contact constraints. **Check** a target that moves, rotates, disappears, or becomes invalid during alignment. Measure hand or foot error and verify the selected cancellation rule.
- **Keep correction observable.** Visualize authored and requested targets, the warp window, and the applied displacement. Give designers limits for excessive stretching and acceleration. **Check** recorded routes at supported frame rates and under hitches. Compare endpoint error, peak correction speed, contact slip, and collision outcomes against the project's tolerances.

Use [Animation root motion](game-animation-root-motion.md) to choose displacement ownership, and [Inverse kinematics](inverse-kinematics.md) for final limb contacts.
