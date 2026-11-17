# Game AI

## Problem and design reasoning

**Problem.** Adding abilities multiplies behavior transitions, scripts make actors respond to information they could not perceive, or threat pacing loses its recovery phase.

**Design reasoning.** Monolith describes composing actions through a planner while keeping execution distinct. Valve describes separately managing encounter pacing. The design inference is to give perception, selection, execution, and pacing their own responsibilities when those concerns need independent tuning.

Specify the behavior players should be able to observe. Separate an actor's knowledge, its choice of action, the execution of that action, and any system coordinating the wider encounter. Each layer has different failure modes and can be inspected on its own.

## Shipped examples

**F.E.A.R. (2005).** Monolith's Jeff Orkin describes dividing behavior between a goal-oriented planner and a small execution state machine. The planner composed actions while the execution layer handled moving, playing animations, and using smart objects. The architecture supported new combinations of actions without authoring a fixed transition for every possible sequence. [Orkin's GDC 2006 slides](https://web.cs.wpi.edu/~rich/courses/imgd4100-b11/lectures/L-Planning.pdf)

**Left 4 Dead (2008).** Valve's Director estimated survivor intensity from gameplay events and modulated the frequency of threats. Its cycle escalated pressure, briefly sustained it, allowed the current encounter to tail off, and then gave the team time to recover. Valve describes this mechanism as controlling pacing. [Michael Booth, The AI Systems of Left 4 Dead](https://steamcdn-a.akamaihd.net/apps/valve/2009/ai_systems_of_l4d_mike_booth.pdf)

**The Last of Us Part II (2020).** Naughty Dog added tracked scent as a distinct sensory channel for guard dogs. Ellie leaves a fading trail they can follow after losing visual contact. Listen Mode exposes the trail to the player, giving them information they can use to change their route. [Druckmann interview](https://blog.playstation.com/archive/2019/10/09/neil-druckmann-discusses-new-the-last-of-us-part-ii-details/) · [PlayStation gameplay preview](https://blog.playstation.com/archive/2019/09/26/hands-on-with-the-last-of-us-part-ii-new-gameplay-details-revealed)

These cases illustrate different scopes of development. An action planner helps an actor combine reusable verbs. An encounter director varies when actors appear. A new sense changes what actors can know and gives the player another rule to learn. Add complexity at the layer that explains the missing behavior.

## Techniques

### Model perception and memory explicitly

Record what a sensor detected, where, when, and with what confidence. Let each sense define range, obstruction, refresh rate, and memory expiry. Maintain a last-known location that can be investigated after contact ends. Expose the rules with cues appropriate to the game, such as a search animation, alert sound, or visible uncertainty marker.

**Check** by breaking line of sight, crossing sensor boundaries, entering overlapping senses, and waiting just past the memory timeout. Trace whether pursuit uses current knowledge or a remembered position. Test that actors resume their intended behavior once the evidence expires.

### Give actions preconditions, outcomes, and exits

For each action, describe the state it requires, the change that counts as success, and how it yields to failure or interruption. Start with explicit states for simple sequences. Use scored selection for several competing actions, or consider planning when goals routinely require multiple interchangeable steps. Use a minimum commitment, score margin, or cooldown where rapid switching makes behavior look indecisive.

**Check** with an action fixture for each precondition, success, interruption, and failure. Remove a destination during execution or reserve it for a second actor. Verify fallback, reservation release, and a bounded number of retries.

### Validate paths and local coordination separately

Represent traversable areas and special links for actions such as ladders or jumps. Use long-range pathfinding to choose a route and local steering to follow it around nearby actors. When a resource has limited capacity, such as an interaction point, give it timed reservations and a fallback destination. Replan when a route becomes invalid or a stuck timer expires, with a limit on repeated requests.

**Check** against narrow doors, moving obstacles, unreachable goals, and groups converging from opposite directions. Log path requests, failures, queue time, and arrival rate. Review the longest stalls and profile the navigation cost in crowded scenes.

### Pace encounters with bounded pressure

Feed a director a few observable signals such as damage, active threats, or progress. Set phase durations and thresholds, plus rules for valid spawn locations, population limits, and authored exceptions. Capture the random seed and log candidates rejected by visibility, reachability, or encounter constraints.

**Check** a slow group, a fast group, a split group where supported, and repeated passes over the same route. Compare time spent under pressure, recovery time, and population peaks. Replay the trace to inspect abrupt phase changes and confirm spawning respects map rules.

### Budget background decisions and trace what players can see

Use one scheduler to track when actors need to think. Give immediate work to actors in active interaction or responding to significant events. Stagger routine updates across ticks and use cheaper updates for distant actors where simulation rules permit. Specify which timers and state transitions remain authoritative in every tier. IO Interactive's crowd work documents treating steering, behavior selection, and visual representation as distinct parts of the system. [IO Interactive's crowd-system talk](https://www.gdcvault.com/play/1015315/Crowds-in-Hitman)

**Check** a dense crowd, rapid camera rotation, abrupt focus shifts, and a quest actor crossing the simulation boundary. Log decision age, queue depth, and worst-case time to respond. Inspect a trace from stimulus through selected action and execution. Confirm that a change in update tier preserves the actor's commitments and that significant stimuli receive a response within the encounter budget.

For concrete actor roles, read [NPCs](npcs.md) for routine and interaction design, [Enemies](enemies.md) for encounter behavior, or [Companions](companions.md) for following and assistance.
