# Game programming patterns

## Problem and design reasoning

Shared gameplay actions acquire inconsistent rules when input, scripted sequences, and AI each implement their own calls. The released *Quake* source shows a small dispatch boundary for named console commands. Its buffer can execute a sequence across frames, and command handlers remain separate from parsing and sequencing. Bungie's *Destiny 2* presentation describes stateless scenes and unified triggering, and the developers credit stateless scenes with avoiding concurrency logic errors in cooperative gameplay. A useful inference is to choose the narrowest pattern that expresses the problem and make state ownership explicit at each boundary. [id Software, Quake command source](https://github.com/id-Software/Quake/blob/master/WinQuake/cmd.c) · [Bungie, Walking, Talking and Projectiles, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Moran_Evan_Walking_Talking_And_Projectiles.pdf)

## Techniques and practical checks

### Commands for requests with independent producers

Give each supported request an explicit type, parameters, and a handler that validates it against current state. Route player and scripted requests through the same handler when they should obey the same rules. Document whether requests execute immediately or enter a queue with an explicit order.

**Check** send identical interaction requests from a binding and a scripted sequence. Compare accepted effects, refusal reasons, and ordering if the target disappears before dispatch.

### State machines for mutually exclusive behavior

Give one owner control of transitions, and define which guards apply, what happens on entry and exit, and which interruptions take priority. Introduce hierarchy only when several states genuinely share transitions.

**Check** trigger retry, pause, and completion in the same update. Verify the expected transition wins and that restarting clears timers, listeners, and temporary restrictions.

### Strategies for replaceable policies

Use a shared contract when multiple algorithms solve the same problem, such as nearest-target and lowest-health selection. Keep the input snapshot and tie-breaking rules visible. Introduce an explicit selection point where the game configures the policy.

**Check** run both strategies against an empty candidate set, an invalid candidate, and two equal candidates. Swap policy using ordinary project configuration and confirm that unrelated behavior is unchanged.

### Events for completed outcomes

Publish a fact after its authoritative update commits, then let audio, UI, or telemetry subscribe according to their own lifetime. Make the producer-to-consumer relationship traceable. Return consequential success or failure from the operation that owns it.

**Check** execute the action with presentation listeners enabled and disabled. Confirm identical gameplay outcomes, one event on success, and no stale delivery after unloading and reloading the scene.

For system ownership across larger features, see [Gameplay architecture](gameplay-architecture.md).
