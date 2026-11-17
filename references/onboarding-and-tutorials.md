# Onboarding and tutorials

## Problem and design reasoning

**Problem.** Players follow prompts but cannot apply the mechanic without help, or a failed first attempt forces enough repetition that they miss the lesson.

**Design reasoning.** Valve revised Portal's Companion Cube chamber after tests showed players kept losing the cube. That response shows using observed confusion to simplify the conditions for learning before increasing pressure.

## Shipped examples

**Super Mario Bros. (1985).** Shigeru Miyamoto describes an onboarding problem with the first mushroom. Its motion could make a newcomer mistake it for a threat and jump out of its way. A pipe turns the mushroom back toward the player, giving them a second chance to discover what it does through play. [Nintendo Iwata Asks, Miyamoto interview](https://iwataasks.nintendo.com/interviews/wii/nsmb/0/3/)

**Portal (2007).** Valve's postmortem describes removing moving lifts and a hazard from an early Companion Cube design after testers kept losing the cube and retracing their steps. The revised chamber keeps the destination button visible and uses dialogue when environmental hints prove insufficient. The team also describes teaching an interaction before asking players to use it under pressure in the finale. [Kim Swift and Erik Wolpaw, GDC 2008 slides](https://cdn.akamai.steamstatic.com/apps/valve/2008/GDC2008_PortalPostMortem.pdf)

**Street Fighter 6 (2023).** Capcom's showcase separates learning basic rules in a tutorial from practicing a particular fighter's style in a character guide. In the shipped World Tour mode, players can enroll with a master and learn their special moves through progression. These give different reasons to return to learning after the first session. [Capcom showcase and mode descriptions](https://news.capcomusa.com/2023/04/20/street-fighter-6-showcase-recap/)

## Techniques and practical checks

### Build a learning sequence around decisions

For each required skill, define the player decision and a signal that shows they understood it. Introduce the skill in a low-pressure task, then present a new layout or situation that asks the player to transfer it. Give each step a quick recovery path and limit how many unfamiliar rules need attention at the same time.

**Check** first-time players against a variant with the same rule but a different layout. Ask what they expect before they act. Track independent success and recovery time separately from completion of the original exercise.

### Escalate help only when it can resolve confusion

Start with a visible affordance. Offer an eligible hint after a meaningful failed attempt or a player request. If the player still struggles, provide a more specific cue, a repeatable demonstration, or practice at a lower pace. Resolve button glyphs from current bindings and clear pending prompts when the player progresses.

**Check** a first attempt, repeated mistakes, a remap, an input-device switch, and resuming after interruption. Verify that players who solve the task independently see little interruption and that requested help remains available after dismissing it.

### Keep lesson state resumable

Store stable lesson identifiers and the distinction between seen, practiced, and completed where those values affect behavior. Define what can be skipped and how to reopen a lesson. At checkpoints, restore the actual game state required for practice, including staged objects or temporary rule overrides.

**Check** quitting or reloading before the cue, during practice, and after success. Verify that one-time rewards are issued once, mandatory gates stay reachable, and previous learners can practice without clearing progression.

### Diagnose the exact step where novices stall

Track first meaningful action, retries, hints, completion, and later transfer. Match events to the lesson and build version. Pair event counts with observed playtests so a pause can be distinguished from a misunderstanding, an unreadable prompt, or experimentation.

**Check** by comparing two tutorial variants with the same downstream challenge. Judge each by independent success and common failure reasons as well as completion rate.
