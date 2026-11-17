# Horror design

## Problem and design reasoning

**Problem.** A fictional threat becomes routine after repeated exposure, or constant stress makes it difficult for players to notice changes in tension.

**Design reasoning.** Creative Assembly's premise gave the player limited power against one creature. A practical inference is to preserve uncertainty and the ability to recover so each return of the threat still matters.

## Shipped examples

- **Alien: Isolation (2014).** Creative Assembly built the experience around one frightening fictional creature and an underpowered player. Alistair Hope describes how the creature's senses and the game's audiovisual world served that premise. [Building Fear in Alien: Isolation, GDC 2015](https://gdcvault.com/play/1021852/Building-Fear-in-Alien)
- **Amnesia: The Bunker (2023).** Frictional's Halloween update introduced settings that vary stalker behavior, the safe room, and save costs. A later update added a Very Easy preset and connected camera shake to the reduced-motion setting. [Halloween update](https://frictionalgames.com/2023-10-amnesia-the-bunker-halloween-update-launch/) · [Version 1.31](https://frictionalgames.com/2024-02-amnesia-the-bunker-updated-1-31/)

## Techniques and checks

### Alternate uncertainty with relief

Draw the intended curve through exploration, anticipation, escalation, and recovery. Assign each space a role on the curve and budget how often intense encounters can occur. Use recovery windows for planning, reading clues, or hearing faint environmental details.

**Check** play the same sequence at a fast and slow pace, then resume from a checkpoint after a failure. Log encounter spacing and ask where players felt pressure or relief. Identify interruptions that crowd out recovery or make an intended reveal routine.

### Keep signals readable and their meaning consistent

Use separate cues for ambience, proximity, and immediate interaction. Let a single cue leave room for uncertainty about what happens next. Provide a second way to convey information necessary to make a decision, such as a caption or a visible reaction.

**Check** test with speakers, headphones, and audio muted with captions or other alternatives enabled. Ask players to distinguish a harmless ambience event from a gameplay threat before they act. Record false positives and missed cues.

### Give fear a decision cost

Connect resources to consequences that matter in the next scene, such as spending light now or conserving it for an unknown room. Provide at least one way to continue when common resources run low. Surface the consequence before committing when a choice could make progress impossible.

**Check** start with typical, near-empty, and overstocked inventories. Verify that mandatory objectives remain reachable from supported checkpoint states and that resuming preserves spent resources and already completed objectives.

### Vary presentation while preserving constraints

Set authored limits on where and when a fictional encounter can trigger. Vary timing or presentation inside those limits. Give cinematics, major clues, and recovery areas explicit encounter policies. Allow players to reduce strong presentation effects while retaining the cues needed to play.

**Check** replay the same route with different seeds and reduced-motion settings. Look for encounters intruding on protected beats, unavoidable outcomes directly after loading, and cue loss when shake or flashes are reduced.
