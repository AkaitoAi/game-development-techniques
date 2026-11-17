# Controller types

## Problem and design reasoning

**Problem.** A click-to-move system feels awkward on a stick, or a device port silently changes reachability and target selection.

**Design reasoning.** Diablo II: Resurrected's designers describe direct analog movement and target ranking for gamepad play while retaining a consequential walk/run choice. That supports choosing a command model around what the device expresses naturally and preserving the underlying rules that matter.

A controller has three independent choices. The command expresses a direction, destination, or discrete action. The movement model resolves it as authored displacement, forces, or a state change. The viewpoint determines the frame of reference for aiming and movement. A gamepad, keyboard, touchscreen, or assistive device can supply several kinds of commands.

## Shipped examples

**Super Mario 64 (1996).** Nintendo documents analog stick direction and deflection as movement direction and speed, with separate camera buttons. This scheme lets one hand continuously steer the avatar while the other adjusts the viewpoint on demand. [Nintendo instruction manual](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/nintendo_8/Manual_Nintendo64_SuperMario64_EN.pdf)

**Diablo II (2000) and Diablo II: Resurrected (2021).** Design director Robert Gallerani describes how the original mouse mode used clicks and pathfinding to reach destinations and perform actions. For the remaster's gamepad mode, the team enabled direct analog movement, ranked candidate targets in a cone, and used an explicit walk/run toggle to preserve a consequential choice. [Blizzard and Vicarious Visions, controller design account](https://blog.playstation.com/2021/08/13/diablo-ii-resurrected-crafting-the-controller-experience/)

The Mario example shows continuous direction on hardware that also offers discrete camera commands. The Diablo remaster shows how the same world and rules can support both destination and direct control while each scheme handles navigation and targeting differently.

## Select the command and motion independently

| Pattern | Use when | Practical check |
|---|---|---|
| Direct first-person or third-person movement | The player regularly chooses a direction, face, or aim. In first person, use a stable aim reference. In third person, choose whether facing follows movement, aim, or lock-on independently. | Walk while looking sideways, circle a target, and rotate the camera during a held input. Check the intended move and facing at each transition. |
| Point-and-click or tap-to-move | The player selects destinations and targets while route execution is delegated. Specify replace versus queue, stopping tolerance, and whether interaction follows arrival. | Click beyond obstacles and into unreachable space, then change destinations while moving. Verify cancellation and clear failure feedback. |
| Grid or turn-based control | Occupancy, action order, and legal moves are gameplay rules. Resolve cell claims and turn cost in simulation, then animate between committed states. | Send two actors to one cell, cancel a preview, and replay a blocked move. Validate occupancy, action cost, and arrival order. |
| Kinematic motion | The game should closely enforce jump arcs, sweep-and-slide locomotion, or authored paths. Own the desired displacement and explicitly handle contacts and external impulses. | Test thin obstacles, early corner hits, moving supports, and interruption by knockback. Verify motion stays inside the intended tolerance. |
| Dynamic rigid-body motion | Momentum, pushes, and contact response are themselves part of play. Apply forces or impulses through the simulation and tune damping and limits. | Test collisions at several masses and speeds, slopes, and a low frame rate. Verify response and motion remain within the supported envelope. |

## Practical checks for hybrid schemes

### Give shared actions a single meaning

Map remappable bindings to actions such as move, interact, aim, and cancel. Normalize analog inputs once. Preserve the distinction between a stick's desired direction and a pointer's requested location, then pass both through the same collision, stamina, and action eligibility rules as appropriate. For assistive schemes, define a way to perform each critical action without requiring simultaneous inputs where the design permits it.

**Check** with remapping, digital diagonals, stick drift, tap and hold, device disconnect, and changing devices during ongoing movement. A change in device should leave the player able to cancel queued actions and reach critical interactions.

### Make ownership transfers observable

Define which source owns motion during direct movement, path-following, scripts, vehicle entry, and any other handoff. Transfer destination, velocity, or facing only when the new mode can interpret it. Cancel incompatible queued requests and expose any important feedback about what took control.

**Check** by taking over midway through a path, switching out of lock-on while moving, and canceling at an interaction boundary. Verify no duplicate interactions, unresolved paths, or stale held commands remain.
