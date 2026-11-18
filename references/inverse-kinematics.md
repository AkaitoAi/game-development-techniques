# Inverse kinematics

## Problem and design reasoning

A recorded animation has fixed contact positions, while game terrain, props, and character proportions vary. Avalanche's *Just Cause 2* presentation combines hand and foot IK with animation and physics to support many stunts within a limited animation budget. Its published update order also demonstrates that pose corrections and physics must agree about which result the next stage reads. [John Fuller and Andreas Nilsson, Physics Meets Animation, GDC 2010](https://media.gdcvault.com/gdc10/slides/Nilsson_Andreas_PhysicsMeetsAnimation.pdf)

Ubisoft's IK Rig presentation describes modifying base poses at runtime in response to obstacles, props, and character state while preserving artistic control. It presents a reusable technology rather than documenting one shipped game's complete solver. Naughty Dog's *Uncharted* postmortem provides shipped-game context for registering a character to the environment through IK. A transferable inference is to define targets, spaces, priorities, and constraints before choosing a solver. [Alexander Bereznyak, IK Rig, GDC 2016](https://gdcvault.com/play/1023279/IK-Rig-Procedural-Pose) · [Naughty Dog Uncharted postmortem, Game Developer April 2008](https://media.gdcvault.com/GD_Mag_Archives/GDM_April_2008.pdf)

## Techniques and practical checks

- **Make target data explicit.** Represent target position, orientation, coordinate space, validity, and blend weight. Record when a moving-surface target was sampled and retain a local anchor where contact should follow that surface. **Check** a rotated parent, moving platform, missing target, and teleport. Visualize both target and effector to expose space conversion errors.
- **Constrain the solution.** Choose a two-bone solver for simple limb reach or an iterative chain solver when the feature needs more joints. Set reach limits, a bend direction, convergence tolerance, and an iteration budget. Define the fallback for unreachable targets. **Check** a target beyond reach, nearly straight limb, target behind the chain, and abrupt direction reversal. Inspect joint limits, flipping, and bounded execution time.
- **Stabilize contacts across frames.** Separate acquiring a contact from keeping it. Use animation contact phases and hysteresis to avoid resampling a planted foot every frame. Release the target when the animation or gameplay ends contact. **Check** stairs, uneven ground, short steps, and a surface that disappears. Measure foot drift and the time to release an invalid contact.
- **Order competing pose corrections.** Document whether pelvis adjustment, foot placement, weapon grip, look-at, and physics operate before or after one another. Give overlapping chains priorities and blend limits so one pass cannot silently undo another. **Check** aiming while crouching on a slope, a two-handed prop, and a transition into ragdoll. Compare effector errors and pose continuity at each pipeline stage.

For runtime scheduling, see [Animation job pipelines](game-animation-job-pipelines.md). For physical handoffs, see [Ragdoll systems](ragdoll-systems.md).
