# Level design

## Problem and design reasoning

**Problem.** Players miss a useful item, overlook a route, or arrive at an encounter without understanding what action the space expects.

**Design reasoning.** Nintendo's mushroom-and-pipe setup gives a second opportunity after a plausible newcomer mistake. The design reasoning is to arrange space so players can learn from the result of an action and recover without breaking the flow.

Design the space around what the player needs to notice, decide, and execute. A playable blockout can test these relationships early, while there is still room to move a doorway, resize a jump, or restructure a route.

## Shipped examples

**Super Mario Bros. (1985).** Nintendo's developers sketched World 1-1 on paper and traced likely player actions through the layout. They placed the first pipe so the initial mushroom would bounce back if it passed the player, creating a second opportunity to see what it does. The opening was produced late and adjusted until the end of development. [Nintendo developer interview](https://www.nintendo.com/en-gb/Iwata-Asks/Iwata-Asks-New-Super-Mario-Bros-Wii/Volume-2/4-Adjusting-the-Map-in-a-Daily-Cycle/4-Adjusting-the-Map-in-a-Daily-Cycle-233027.html)

**The Last of Us Part II (2020).** Creative director Neil Druckmann describes larger layouts used to vary tension and encourage exploration. Some sequences allow players to be detected, engage, and escape the area, while others allow them to avoid detection throughout. The encounter's exit and completion requirements are therefore part of the design of each route. [Druckmann interview on PlayStation Blog](https://blog.playstation.com/archive/2019/10/09/neil-druckmann-discusses-new-the-last-of-us-part-ii-details/)

**Super Mario Bros. Wonder (2023).** The team prototyped courses that transform after the player triggers a Wonder effect, including a prototype based on bending and twisting pipes. The developers describe preserving normal pipe behavior as a familiar expectation and grouping dramatic changes under a recognizable Wonder presentation. [Nintendo developer interview, chapter 1](https://www.nintendo.com/au/news-and-articles/ask-the-developer-vol-11-super-mario-bros-wonder-chapter-1/)

The cases span a fixed opening, freely approached encounters, and courses that change during play. The first still offers a cheap test for the others. Trace what happens when the player takes the unexpected branch, misses a cue, or leaves at the earliest available exit.

## Techniques

### Block out the route before detailing it

Draw a graph of entrances, decisions, landmarks, shortcuts, and exits. Replace the graph with a simple playable layout using the real character dimensions, camera, and movement limits. Measure travel time as well as geometric distance. Label edges with any abilities, keys, scripted states, or one-way drops they require.

**Check** every required destination from each legal entrance. Test forward traversal, backtracking, checkpoint reload, and entering with the least capable allowed character. For a one-way drop, confirm that the player can still finish or recover without restarting.

### Teach with geometry and room for recovery

Isolate a mechanic at a low-cost obstacle. Make its cause and outcome visible from the approach, build a retry into the layout, then vary one condition in the next challenge. For a moving platform, provide a stable place to watch a full cycle and a nearby return path when the player misses it.

**Check** with first-time players. Log the first attempt, the point where they discover the rule, and whether they apply it in a changed arrangement. Test the second obstacle before deciding that success on the first demonstrated understanding.

### Give each approach a readable decision

At a junction, expose a spatial cue the player can recognize from the decision point, such as a distinctive silhouette, a visible route connection, or activity framed through an opening. Use safe overlooks for planning when the player's choices depend on seeing a large area. Set up routes with different costs or rewards, such as slower cover versus a short, exposed crossing.

**Check** from all legal approach directions, under shipping lighting and camera settings. Ask unbriefed players to point out where they expect each route to lead. Record hesitation, wrong turns, and which routes they reach after the encounter changes state.

### Test pacing and transformations as state changes

Map where pressure builds, peaks, and releases along each major route. If the space transforms, identify what happens to traversal, hazards, exits, checkpoints, navigation, and player positions at the transition. Give each state a distinct cue and define when the previous layout returns.

**Check** a normal traversal, a fast run, and a cautious run with reversals. Trigger the change near walls and map boundaries, while airborne or alongside another player where supported, and reload after the change. Verify that gates, exits, and checkpoints agree with the visible layout and that the player always has a valid recovery path.
