# Non-player characters

## Problem and design reasoning

**Problem.** NPCs miss appointments, repeat conflicting dialogue, or lose task state when an area unloads. A player cannot plan an interaction if availability has no readable explanation.

**Design reasoning.** Nintendo exposes schedules in the Bombers' Notebook, and Warhorse describes persistent daily routines. Their approaches suggest treating presence as a gameplay commitment with visible signals, plus a fallback when a simulated routine changes.

Start by specifying what the player can learn from an NPC, what services or quest actions it owns, and which facts must survive leaving the area. A background passerby, a shopkeeper, and a quest witness can use different simulation budgets.

## Shipped examples

**The Legend of Zelda: Majora's Mask (2000).** The Bombers' Notebook records appointments and schedules, and tracks completion of character-related events. Making time and availability visible lets the player plan a route through the repeated three-day cycle. [Nintendo instruction booklet, pp. 24–25](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/nintendo_8/Manual_Nintendo64_TheLegendOfZeldaMajorasMask_EN.pdf)

**Kingdom Come: Deliverance II (2025).** Warhorse presented daily schedules for persistent inhabitants and discussed how interlocking systems govern interactions such as an NPC asking a player to leave a room. The publisher's post-launch notes report changes to schedules across both maps and fixes to specific local routines. The two sources document both the simulation goal and the need to validate individual inhabitants in context. [Warhorse talks at GDS Prague](https://2024.gdsession.com/index.php?action=kcd) · [Deep Silver patch 1.2 notes](https://www.deepsilver.com/games/kingdom-come-deliverance-ii/news/patch-12)

The cases show different uses for the same piece of data. An activity window can create a deliberate appointment or give a settlement predictable daily rhythms. Both depend on players being able to find the character when a required interaction is available.

## Techniques

### Author schedules as activities with fallbacks

Give each activity a time window, destination or region, capacity, and fallback. Define precedence for a scene, a world interruption, and routine work. Resume the activity that is valid at the current world time when an override ends. Use visible cues for critical appointments, such as a shop sign, a calendar entry, or dialogue that names the next location.

**Check** just before and after each boundary, across midnight, after time skips, and with a closed door or occupied destination. Verify that missed optional appointments follow the intended rules and any required step still has a recovery path.

### Keep persistent identity independent of scene instances

Use stable character IDs for relationships, inventory ownership, quest references, and recorded outcomes. A loaded actor presents that record. Outside the active area, advance only the durable facts that matter, such as the next location or a timed quest event. Restore actor state before exposing the character's interaction prompts.

**Check** by leaving mid-routine, returning after several schedule changes, and saving and loading at a world boundary. Expect one actor per persistent ID, conserved inventory, and identical quest consequences across unload and reload.

### Resolve dialogue and services from shared facts

Give greetings, services, and quest conversations explicit predicates and priorities. Query the same availability rule for prompts and interaction execution. For a quest step that consumes an item or unlocks a service, commit the outcome once and store a stable completion flag.

**Check** repeated interactions and interruptions at the commit point. Verify alternate entry points, mutually exclusive lines, missing prerequisites, and loading a save made immediately before or after a transition.

### Validate a settlement through ordinary play

Build a small test settlement with a meeting point, a crowded doorway, a workplace, and a long route. Trace each important character's schedule target, actual position, current override, waiting time, and interaction availability. Stagger expensive path and decision updates when population increases.

**Check** with observers arriving at peak and quiet hours. Compare missed appointments, stalled trips, and worst-tick cost between normal play, a fast clock, and a crowded fixture. Review whether players can infer where to go from the cues they actually see.
