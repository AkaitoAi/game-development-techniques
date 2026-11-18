# Traversal route design

## Problem and design reasoning

A level can have valid jumps and still offer an unclear or unrecoverable route. The design task is the sequence of approach, commitment, landing, and next choice. The GDC Europe 2010 session *The Tao of Level Design* compares challenges from shipped games including *Mirror's Edge* and *Alan Wake*. Its public summary supports treating methods as dependent on a game's demands; it does not establish a universal traversal metric. [Neil Alphonso, Saku Lehtinen and Elisabetta Silli, GDC Europe 2010](https://gdcvault.com/play/1013686/The-Tao-of-Level-Design)

Sucker Punch's environment lead describes an intent for visible destinations in *Ghost of Tsushima* to be reachable with few exceptions. The studio built distinct biomes and many points of interest around that world. A portable inference is that a destination's visual promise should agree with its approach route. The recommendations below concern authored route decisions and recovery. [Joanna Wang, Crafting the world of Tsushima](https://blog.playstation.com/2020/07/09/crafting-the-world-of-tsushima/)

For geometry dimensions, see [Level blockout and metrics](level-blockout-and-metrics.md). For ledge execution, see [Climbing and mantling](climbing-and-mantling.md).

## Techniques and practical checks

### Specify the sequence of commitments

Draw the route as approach, preview point, commitment, landing, and exit. Label each segment with the abilities and approach speed it requires. Place a decision before a player loses the ability to change direction.

**Check** run the route at the lowest and highest legal approach speeds and with each supported movement configuration. Record the first irreversible action and confirm the next safe destination is visible or already taught before that point.

### Expose actionable landmarks

Use silhouettes, openings, surfaces, or movement to identify the next useful anchor. Keep decoration consistent with the game's traversal rules. A nearby decorative ledge can otherwise compete with the intended route.

**Check** ask unbriefed players to identify their next landing or handhold from each decision point. Repeat under shipping lighting and camera settings, and investigate a recurring wrong prediction before adding another prompt.

### Author recovery alongside success

Decide where an early release, weak jump, or late turn sends the player. Use a safe lower route, a short retry, or a known checkpoint when the game allows it. Confirm that an optional branch can rejoin or terminate clearly.

**Check** deliberately fail each commitment at its early and late boundary. Verify the player can reach a valid retry or continue path and that recovery does not skip a required ability gate.

### Evaluate routes after encounter changes

Trace the route when doors close, hazards activate, props move, or the player approaches backward. Review any required handoff between traversal and combat against the actual camera and control state.

**Check** replay a fast run, a cautious run with reversals, and a run interrupted at each state change. Record forced stops, unclear exits, and invalid recovery positions. Treat the route's timing and readability as measured outcomes.
