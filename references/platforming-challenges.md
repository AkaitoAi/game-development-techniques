# Platforming challenges

## Problem and design reasoning

**Problem.** Players can execute a jump in isolation yet miss it when camera framing, moving hazards, or a distant checkpoint obscure the timing. Watch where failures cluster and how long it takes to retry.

**Design reasoning.** Nintendo recounts using the opening pipe to return a missed mushroom. An inference from that example and Celeste's staged rooms is that geometry can provide a second chance to learn while later rooms test mastery. See the shipped examples below for sources.

## Shipped examples

**Super Mario Bros. (1985).** Nintendo's developers describe placing the first pipe so the mushroom can bounce back toward Mario. The opening uses geometry to give a new player another chance to encounter a moving power-up. The team made and revised World 1-1 late in development, after working on later areas. [Nintendo developer retrospective](https://www.nintendo.com/en-gb/Iwata-Asks/Iwata-Asks-New-Super-Mario-Bros-Wii/Volume-2/4-Adjusting-the-Map-in-a-Daily-Cycle/4-Adjusting-the-Map-in-a-Daily-Cycle-233027.html)

**Celeste (2018).** Maddy Thorson's GDC 2017 workshop describes the tools and process used to make hundreds of platforming stages and arrange them into the mountain's areas. The staged approach gives designers places to isolate a hard maneuver, vary it, and test how the sequence fits the area's rhythm. [Maddy Thorson, Level Design Workshop, GDC 2017](https://www.gdcvault.com/play/1024307/Level-Design-Workshop-Designing-Celeste)

## Techniques and practical checks

### Measure practical reach from the shipped controller

Build a test space with movable takeoff and landing geometry. Measure reachable landings at different launch speeds, elevations, approach lengths, and jump release times. Sample likely player inputs to measure the usable landing window. Reserve tight margins for challenges that intend to ask for precision.

**Check** each required jump with the minimum legal movement state and with representative input variation. Log where a short press, late press, or early turn lands. Test optional movement assists using the same fixtures.

### Teach one action through geometry

Introduce a mechanic where there is safe space to discover its timing. Show the takeoff, expected landing, and relevant moving hazard before the player commits. Carry the mechanic into a second layout that changes a single constraint before asking the player to combine it with older skills.

**Check** initial attempts with players unfamiliar with the level. Classify misses by route choice, awareness of the hazard, and input timing. Ask the player to attempt the second layout to see whether the lesson transfers.

### Design the retry loop

Choose checkpoint positions with the time and inputs needed to retry in mind. Restore moving surfaces and hazards to a consistent or readable phase. Make failure causes visible and leave sufficient space for the next attempt's first input.

**Check** failures from multiple points in a room. Record the time until input returns, the time until the first difficult action, and whether respawn recreates a safe setup. Repeat when the player holds an action through the reset.

### Test compositions and alternative routes

Add a new demand by changing one variable such as crossing direction, moving platform phase, or recovery space. Mark optional shortcuts and verify their intended cost. Check whether a player who reaches a route by a different approach has the movement resources needed for the next mandatory action.

**Check** rooms at different entry speeds, on repeated attempts, and with camera or control options enabled. Review failed attempts for edge collisions, unreadable crossings, and legal landings that leave the player stranded.
