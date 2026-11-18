# Ragdoll systems

## Problem and design reasoning

Authored reactions can repeat visibly, while an unconstrained switch to physics can lose the character's momentum, silhouette, and control. Naughty Dog added physics over gameplay animation in *Uncharted 4* to gain fluidity and reduce the number of authored assets, then developed more predictable animation-driven physics to retain artistic control. [Michal Mach, Physics Animation in Uncharted 4, GDC 2017](https://gdcvault.com/play/1024087/Physics-Animation-in-Uncharted-4)

In *Just Cause 2*, Avalanche combined animation, hand and foot IK, ragdolls, and aim constraints. Its published control flow and transition states make ownership explicit, with pose matching used when returning from ragdoll. A transferable implementation inference is to represent animation, driven physics, full ragdoll, and recovery as distinct modes with tested handoffs. [John Fuller and Andreas Nilsson, Physics Meets Animation, GDC 2010](https://media.gdcvault.com/gdc10/slides/Nilsson_Andreas_PhysicsMeetsAnimation.pdf)

## Techniques and practical checks

- **Define mode ownership.** Specify which system owns the collision body, root transform, and each pose channel in animated, driven, full-physics, and recovery modes. Configure collision filters so the controller capsule and simulated limbs behave deliberately together. **Check** switching modes near a wall, on stairs, and during a moving-platform ride. Inspect root discontinuity, interpenetration, and duplicate collision responses.
- **Initialize from the evaluated pose.** Transfer bone transforms at a defined simulation boundary and estimate linear and angular velocities from recent pose samples when the engine supports this. Clamp exceptional values after teleports and define how stale samples reset. **Check** activation during a run, fall, sharp turn, and teleport. Verify the body continues plausible momentum without an explosive first step.
- **Bound the driven simulation.** Author mass distribution, joint limits, motor strengths, damping, and allowed stretch against the supported rig. Use quality tiers with explicit gameplay consequences for reduced simulation. **Check** repeated impulses, extreme supported poses, and several simultaneous ragdolls. Measure joint error, maximum penetration, and high-percentile physics cost on target hardware.
- **Recover through a validated transition.** Choose a get-up pose from body orientation, align it to a legal position, and blend from a captured simulated pose. Re-enable navigation and control at an explicit point and preserve the intended state if recovery is blocked. **Check** face-up and face-down landings, slopes, low ceilings, and interruption during get-up. Verify a valid fallback and consistent control timing.

For rig authoring, see [Animation rigging](animation-rigging.md). For contact timing, see [Physics substep events](game-engine-substep-events.md).
