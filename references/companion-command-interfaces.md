# Companion command interfaces

## Problem and design reasoning

**Problem.** A companion can have useful abilities that players rarely use because orders take too long, targets are ambiguous, or the ally gives no feedback when it cannot comply. Autonomous assistance can also override an intentional order unless the game defines how the two interact.

**Documented reasoning.** Santa Monica Studio's *God of War* talk places player commands above two tiers of autonomous actions and identifies ease of use and advanced depth as design goals. Aimed and unaimed input can share one command while using different target selection. BioWare exposes a wider vocabulary and configurable squad power use in *Mass Effect Legendary Edition*. EA's reference documents the controls and settings. [Hayato Yoshidome, Raising Atreus for Battle in God of War, GDC 2019](https://gdcvault.com/play/1026467/Raising-Atreus-for-Battle-in) · [EA accessibility and settings reference](https://www.ea.com/able/resources/mass-effect-legendary-edition/pc)

**Inference.** A direct input fits frequent, urgent help when the current target is legible. An explicit squad interface fits orders whose value depends on selecting a specific ally, target, or destination.

## Shipped-game examples

- **God of War (2018).** Atreus can shoot toward the aimed reticle or select a target when the player commands a shot without aiming. A held input requests a runic summon. Santa Monica Studio discusses rate and ammunition limits as part of balancing the directed shots. [Santa Monica Studio GDC slides, Directed Actions](https://media.gdcvault.com/gdc2019/presentations/Yoshidome_Hayato_RaisingAtreusFor.pdf)
- **Mass Effect Legendary Edition (2021).** The documented PC controls include issuing move or attack orders to individual squadmates and rallying them. Squad power usage also gives players some control over autonomous behavior. [EA controls and gameplay settings](https://www.ea.com/able/resources/mass-effect-legendary-edition/pc)

## Techniques and practical checks

### Make scope and target selection visible

Define the recipient, action, and target rules for every command. Show which companion or group receives the order and which object or position the game resolves. Keep a fast input for frequent actions and offer explicit selection where the chosen target changes the outcome.

**Check** two adjacent eligible targets and an invalid object near the reticle. Try aimed, unaimed, and explicit commands with each supported device. Confirm that the selection cue and executed order agree.

### Model order acceptance and execution separately

Track whether a request was rejected, accepted, running, completed, interrupted, or expired. Acknowledge player input promptly and surface actionable reasons for failure, such as an unreachable destination or a cooldown. Keep resource consumption tied to a defined commitment point.

**Check** orders while the companion is busy, unavailable, on cooldown, or cut off by changing geometry. Verify that each case produces the expected feedback and that canceled or rejected orders charge resources only if intended.

### Define priority, replacement, and cancellation

Specify when an order overrides autonomy, when a safety or story constraint interrupts it, and when the companion returns to baseline behavior. Distinguish repeatable actions from durable orders such as holding a position. Bound queues and make order replacement explicit.

**Check** rapid orders, cancel followed by reissue, a scripted interruption, and a target disappearing before execution. Inspect the command trace for obsolete work, duplicate effects, and an appropriate return to autonomous behavior.

### Test comprehension under gameplay pressure

Teach one core command in a low-pressure situation. Use distinct confirmation and failure cues that hold up in noisy combat, and keep critical commands reachable through supported remaps or alternatives.

**Check** whether first-time players can select the ally, explain what it is doing, and recover from an invalid command. Repeat with audio muted and with a remapped input. Record time to issue a valid order and whether players can identify its outcome.
