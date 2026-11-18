# Moving platform programming

## Problem and design reasoning

A character can slide off a rotating platform, inherit motion twice on a jump, or penetrate a ceiling when a lift rises. These failures usually involve disagreement about the support frame, collision order, or which system owns displacement.

In shipped *Quake III Arena*, mover code keeps the previous states of pushed objects, applies translation and rotation around the mover, and tests their resulting positions. A blocked rider can remain at its original position if that position is clear, losing its support relationship. That documents a concrete policy for separating riding from pushing and recovering blocked movement. [id Software, mover implementation](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/g_mover.c)

Naughty Dog's developer session on *Uncharted: Drake's Fortune* and *Uncharted 2: Among Thieves* describes the relationship between player control and layered animation. Its published overview establishes the movement and presentation context. The reference-frame techniques below are portable recommendations, rather than an attribution of a specific platform algorithm to Naughty Dog. [Travis McIntosh, Animation and Player Control in Uncharted, GDC](https://gdcvault.com/play/1012852/Animation-and-Player-Control-in)

## Techniques and practical checks

### Retain a support frame

Record the supporting body's stable handle and a contact anchor in its local space. For a supported actor, compute where that anchor moved between simulation ticks and apply the support displacement once through the controller's collision path. Use the actual simulation transforms for collision and interpolate presentation separately. Revalidate support after resolving movement.

**Check** idle and walk on translating, rotating, and combined-motion platforms. Place the actor near the rotation center and near its edge. Track local anchor drift and verify platform motion is applied exactly once at different render rates.

### Define detachment velocity

Specify what linear and rotational point velocity a jump inherits. Express this as a change of movement frame and track whether stored velocity is relative to support or world space. Clear the support handle when jumping, falling, or entering another movement mode.

**Check** jump forward and backward from an accelerating platform and from the outer edge of a rotating disc. Compare world trajectories and verify that detachment does not add support velocity twice or discard it unexpectedly.

### Resolve blocked movement deliberately

Choose a stop, detach, displacement, or fictional crush policy for a platform that pushes an actor into an obstacle. Sweep the actor along the intended displacement and validate any recovery position. Keep blocked-mover behavior visible to the platform controller so the platform and rider agree on the accepted move.

**Check** a rising lift beneath a low ceiling, a sliding trapdoor, and two platforms converging on an actor. Verify stable recovery, bounded collision work, and one gameplay consequence for each blocked event.

### Handle disappearing and discontinuous supports

Define behavior when a platform unloads, teleports, changes authority, or is destroyed. Treat a large discontinuity as an explicit relocation or detachment according to the level's rules. In a networked game, reconcile support identity and relative position using compatible simulation timestamps.

**Check** destroy the supporting object, teleport it, and reload a checkpoint during travel. Confirm the character exits support cleanly, lands in a valid location, and avoids a one-frame motion spike.

See [Player controllers](player-controllers.md) for the movement-state boundary and [Game transform hierarchies](game-transform-hierarchies.md) for transform-space ownership.
