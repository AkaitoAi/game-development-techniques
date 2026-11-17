# Animation state machines

## Problem and design reasoning

**Problem.** A clip viewer can make every animation look correct while live play still produces oscillating transitions, late interruption, or contradictory upper- and lower-body poses. As the number of actions grows, developers need a way to know who owns each motion channel and why a transition occurred.

**Documented reasoning.** Naughty Dog describes authoring action-centered states with guarded, data-defined transitions so changes could be made without code edits. Ubisoft's Watch Dogs presentation frames a shared gameplay animation architecture as a way to reuse features across player, combat, and crowd characters. A transferable inference is to make state boundaries match meaningful behavior changes and handle continuously varying signals within the current state. [Naughty Dog, GDC 2008 slides](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf) · [David Therriault, In Your Hands: The Character (of Watch_Dogs), GDC 2015](https://gdcvault.com/play/1022074/In-Your-Hands-The-Character)

## Shipped examples

- **Uncharted: Drake's Fortune.** Animation states were organized around character actions, with transitions specifying destination, blend time, blend curve, and guards. Separate animation information drove blend inputs such as movement and aim angle. [Jeremy Lai-Yates and Judd Simantov, Uncharted Animation, GDC 2008](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf)
- **Watch Dogs.** Ubisoft describes a multi-layered animation architecture made from dynamic trees and replicated state-machine logic. The talk uses combined traversal, phone use, and other actions to illustrate concurrent requirements in a systemic open world. [David Therriault, Ubisoft Montreal, GDC 2015](https://gdcvault.com/play/1022074/In-Your-Hands-The-Character)

## Techniques and practical checks

### Set a transition contract

For each edge, record its trigger, guard conditions, priority, blend time, and interruption policy. Separate facts such as grounded or action-authorized from continuous pose parameters such as speed. Add hysteresis between entering and leaving nearby thresholds when jitter can toggle a state.

**Check** slowly vary speed across both thresholds, then deliver an action request on the same tick as a landing. Record the evaluated guards, selected edge, rejected alternatives, and first visible pose. Verify repeatable outcomes for identical state and inputs.

### Assign layer ownership

Divide by responsibility, such as base locomotion, upper-body interaction, face, and full-body override. Define which layer owns each bone mask and whether its output replaces or adds to the previous pose. Decide who owns world displacement for each action so the controller and root motion apply it once.

**Check** a moving character beginning a partial-body action, taking a full-body interruption, and recovering locomotion. Inspect support-foot motion, spine continuity, and measured displacement. Confirm the interrupted layer releases ownership even if its target clip never completes.

### Define what happens to events when a state exits

Place gameplay authority in the simulation where possible. For presentation markers, track crossings with the playback instance and loop count so they can fire once at the intended point. Write explicit rules for early cancellation and exceptionally fast catch-up.

**Check** interrupt a clip before, at, and just after an event marker. Repeat at slow motion and with a frame hitch that crosses the marker. Compare expected action outcomes and the counts of presentation events.

### Test coverage with an inspectable transition trace

Create a small diagnostic scene with forced states, input replay, current and destination state, layer weights, normalized clip time, and transition reason. Validate the graph for states that have no legal exit and check required transitions in the packaged build.

**Check** each state against the intended action and interrupt matrix. Replay rapid direction changes, repeated requests, and an interrupted blend. Verify entry and exit behavior and record whether repeated transition evaluation fits the intended runtime budget.
