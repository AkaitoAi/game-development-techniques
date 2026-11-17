# Racing track design

## Problem and design reasoning

**Problem.** Drivers repeatedly miss a turn, or one line dominates because alternative lines offer neither speed nor position. Test both navigation and competition at race speed.

**Design reasoning.** Nintendo explains adding direction cues at crossroads in Mario Kart World. Turn 10's staged course process suggests a separate inference that layout, traffic, and alternate lines should be reviewed before visual polish makes changes expensive. See the shipped examples below for sources.

## Shipped examples

**Forza Motorsport 4 (2011).** Turn 10's production talk shows an authored course moving from reference and prototyping through rough and refined layouts, tuning, and polish. The team used staged reviews and QA gates. Track-dependent work can therefore be reviewed against the constraints of the current iteration while the layout is still inexpensive to change. [Turn 10, Racing to the Finish, GDC 2012](https://media.gdcvault.com/gdc2012/slides/Production%20Track/Shek_Arthur_Racing_to_the.pdf)

**Gran Turismo 7 (2022).** Polyphony Digital's CEDEC session covers measurement and data applications for reproducing real circuits alongside appearance design for fictional courses, including Kyoto Driving Park. This gives a team different validation targets for a replica and an original track. [Polyphony Digital's Visual Computing 2022 presentation, CEDEC session summary](https://s3.amazonaws.com/gran-turismo.com/pdi_publications/VC2022_PDI.pdf)

**Mario Kart World (2025).** Nintendo moved from independently built courses to an interconnected world. The team explains that some race routes need explicit direction cues at crossroads so drivers can follow the designated route. Explore navigation needs separately in free driving and timed competition. [Nintendo developer interview, chapter 1](https://www.nintendo.com/en-gb/News/2025/May/Ask-the-Developer-Vol-18-Mario-Kart-World-Chapter-1-2832687.html) · [Chapter 4](https://www.nintendo.com/us/whatsnew/ask-the-developer-vol-18-mario-kart-world-part-4/)

## Techniques and practical checks

### Block out around the vehicle envelope

Author a corridor with width, grade, camber, and surface assignments. Prototype corners and crests with the intended vehicles and chase cameras. Use in-game traces to relate entry speeds and corner radii to steering and braking demand. Keep geometric reference, if required, alongside an independently reviewable gameplay blockout.

**Check** the fastest and slowest vehicle classes on a cold lap and a flying lap. Compare turn-in, minimum speed, exit speed, and collision or off-track incidents by sector. Visualize landmark positions from the actual camera at each approach speed.

### Build overtaking and recovery space

Place passing windows where two drivers can see one another and choose distinct playable lines. Make shortcut tradeoffs intentional through time, handling difficulty, or entry constraints. Leave recovery space where a typical miss would otherwise cause a large delay or force a reset.

**Check** side-by-side approaches and traffic bunching with humans or representative opponents. Time alternate lines with equivalent entries, then inspect collision frequency, reset cost, and whether one option dominates across conditions.

### Make course progress explicit

Define legal connectors and an ordered checkpoint graph with branch joins. Use the visible course and direction cues to tell drivers which route the event uses. Save the last valid progress state for recovery and keep it separate from the nearest geometric point on the course.

**Check** a legal shortcut, missed gate, reversed crossing, reset near a fork, and two consecutive laps. Confirm that position, sector times, and lap count stay correct when nearby segments overlap in world space.

### Validate readability in race conditions

Give major turn types recognizable early cues and test how their placement changes with speed and camera height. Repeat in different weather, lighting, and traffic, including in split-screen when supported. Ensure visual effects and crowds leave the relevant course markers readable.

**Check** first-time drivers approaching junctions and blind crests. Compare missed-route rate and time between the first readable cue and turn-in. Test the same segment behind another vehicle and after a respawn.
