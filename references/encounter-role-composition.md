# Encounter role composition

## Problem and design reasoning

An encounter with individually readable enemies can still produce a confusing combination. Several enemies may demand the same response and make variety cosmetic, or require mutually incompatible reactions at the same time. This guide concerns the combinations players face. [Level encounter scripting](level-encounter-scripting.md) covers how an encounter starts, advances, and retires.

In *DOOM (2016)*, id Software organized combat around aggression and movement, with enemies supplying resources that the player must actively acquire. Kurt Loudy and Jake Campbell explain the reasoning behind this push-forward model. Composition therefore includes the opportunities needed to sustain that loop alongside the threats. [id Software, Embracing Push Forward Combat in DOOM, GDC 2018](https://www.gdcvault.com/play/1024940/Embracing-Push-Forward-)

In *Marvel's Guardians of the Galaxy (2021)*, Eidos-Montréal describes embracing combat's chaotic nature while supporting four AI companions and varied enemy archetypes. The talk establishes coordination as a problem spanning the encounter. The role matrix and pressure checks below are portable design recommendations, rather than a claim that either developer used this exact process. [Eidos-Montréal, Controlled Chaos: The Combat of Marvel's Guardians of the Galaxy, GDC 2022](https://www.gdcvault.com/play/1027917/AI-Summit-Controlled-Chaos-The)

## Techniques and practical checks

### Describe the response each role demands

Give each archetype a primary decision it creates, such as approaching, moving sideways, breaking line of sight, interrupting, or changing target. Record the cue and time available for that decision. Treat damage, speed, and health as supporting parameters. Two visually different units can still fill the same role.

**Check** play each unit alone and ask an unfamiliar player to name the cue and response. Then pair units with the same response demand and a contrasting pair. Record whether the second unit creates a new decision or merely repeats the first.

### Compose a pressure budget

Choose how many simultaneous demands the encounter should present. Limit the overlap of attacks that require precise reactions, and provide openings in which players can recover positioning or resources. Adapt composition to the actual sightlines and traversal routes.

**Check** replay a dense fixture with the lowest permitted resources and from each legal entry. Mark intervals where required responses conflict. Change timing or composition and verify that at least one intended response remains available.

### Include support and counterplay roles

A support unit can change target priority by restoring allies, protecting an area, or enabling another enemy. Make the dependency legible and give the player an opportunity to act on it. Define what happens when the support unit disappears or its partner cannot reach its task.

**Check** remove the support first, leave it until last, and isolate it behind geometry. Verify that each resulting state remains understandable and completable, and that its partners recover from the lost dependency.

### Validate variation against the intended skill

Vary one role, timing, or spatial arrangement at a time while measuring target switches, movement, resource use, and deaths. Compare outcomes with observations of what players recognized. A higher failure rate alone cannot distinguish a learning challenge from a hidden or contradictory demand.

**Check** compare a baseline encounter and one changed composition with representative players. Ask them to explain their last failure and next strategy. Keep the variant when the observed decisions match the intended challenge and retry cost fits the encounter.
