# Encounter recovery design

## Problem and design reasoning

A recoverable mistake can still become a prolonged failure spiral if the player lacks a route, resource, or readable moment to regain control. This guide concerns recovery during a live encounter. [Checkpoints and respawn design](checkpoints-and-respawn-design.md) covers restarting after failure, and [Encounter role composition](encounter-role-composition.md) covers the combination of threats.

In *DOOM (2016)*, id Software framed combat around moving toward enemies to acquire resources. The team describes aggression and speed as the intended player behavior. Recovery therefore participates in the central loop. In *Left 4 Dead*, Valve describes a Director that varies encounter frequency based on team intensity and creates recovery periods. These shipped examples provide two different levers: recovering through action and recovering through a change in pressure. The fixtures below are portable design recommendations. [Kurt Loudy and Jake Campbell, Embracing Push Forward Combat in DOOM, GDC 2018](https://gdcvault.com/play/1024940/Embracing-Push-Forward-Combat-in) · [Michael Booth, The AI Systems of Left 4 Dead](https://steamcdn-a.akamaihd.net/apps/valve/2009/ai_systems_of_l4d_mike_booth.pdf)

## Techniques and practical checks

### Define the recoverable state

List the low-resource states the encounter should allow a skilled player to escape. Specify the available action, space, and information required for that escape. Distinguish a difficult but recoverable position from a state whose remaining outcome is already decided. Establish whether the design permits retreat, resource acquisition under pressure, assistance, or a temporary reduction in threats.

**Check** start the encounter at each supported low-resource state and attempt the intended recovery with the normal controls. Record success, time exposed, and the resource gained. Inspect cases where a player understands the plan but has no viable action.

### Put recovery on a readable route

Place an opening or resource so players can identify a route while handling the current threat. Account for camera direction, traversal time, and the chance that another enemy blocks the route. If a recovery action exposes the player to risk, signal both the opportunity and its cost.

**Check** enter the low-resource state facing several legal directions. Ask unbriefed players to point out their next action, then observe the route they take. Compare recognition failures with execution failures before adjusting resource quantity.

### Preserve an escape from escalating pressure

A new wave, stun, displacement, or support enemy can close a recovery window. Define which overlaps are allowed and which conditions delay more pressure. Apply recovery rules consistently enough that players can learn them, while preserving the challenge the encounter intends.

**Check** trigger reinforcements during a recovery attempt and repeat with a blocked route or unavailable ally. Verify at least one intended route survives in states that the design labels recoverable. Log overlapping demands and their duration.

### Test incentives across skill levels

A recovery reward can become the safest dominant action or encourage deliberate low-resource play. Tune its cost and availability against both first attempts and expert loops. Track why a player recovers, what they sacrifice, and whether the recovered state supports the next meaningful decision.

**Check** compare novice and experienced players using the same encounter. Record recovery attempts, repeated loops, damage taken, and time spent unable to act. Keep successful comeback behavior distinguishable from a resource-farming stall.
