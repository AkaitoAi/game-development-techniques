# Navigation local avoidance

## Problem and design reasoning

A valid route can still run into a prop, a doorway queue, or another moving actor. Rebuilding the route for every nearby change spends pathfinding time and can produce abrupt motion. Michael Booth's *The AI Systems of Left 4 Dead* describes reactive path following toward a look-ahead point, combined with local obstacle avoidance. Valve reports fluid motion and cheaper repathing, with a trade-off that avoidance can take an actor far enough from the route to require a new path. [Valve, Michael Booth, GDC 2009](https://steamcdn-a.akamaihd.net/apps/valve/2009/ai_systems_of_l4d_mike_booth.pdf)

Valve's released Source SDK NextBot path follower separately implements avoidance for nearby obstacles. It provides shipped-code context for the split between route following and local adjustment. The protocols below are portable implementation recommendations rather than a claim that Valve uses every listed rule. [Valve Source SDK, NextBotPathFollow.cpp](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/NextBot/Path/NextBotPathFollow.cpp)

## Techniques and practical checks

### Separate route intent from feasible movement

Compute a preferred velocity from a look-ahead point on the route. Let avoidance propose a nearby feasible velocity, then let locomotion enforce collision and acceleration limits. Preserve the current route and goal while making short adjustments. Give one component ownership of the final movement request.

**Check** a straight corridor containing one movable prop. Trace preferred, avoided, and actual velocities. Verify the actor clears the obstacle, returns toward the route, and retains the intended goal.

### Bound neighborhood work

Query nearby actors through the existing spatial index. Set a range appropriate to speed and stopping distance, cap candidate work, and prioritize imminent conflicts. Use a consistent snapshot of neighbor positions and velocities for an update. Include actor radii and supported movement constraints.

**Check** two crossing groups and a dense stationary crowd. Measure neighborhood query time, candidates processed, penetrations, and throughput. Confirm that the chosen cap still handles the nearest imminent collision.

### Detect lack of progress

Track progress toward the route or goal over a time window. If an actor oscillates or remains blocked, request a bounded recovery such as yielding, choosing another local target, or replanning. Define narrow-passage priority or reservations where free steering repeatedly deadlocks.

**Check** two actors approaching a one-person doorway from opposite sides. Repeat with different arrival times and sizes. Verify eventual passage and a bounded number of replans.

### Preserve traversal and animation contracts

Suspend ordinary steering when a traversal action owns displacement. When that action ends, refresh the route position and avoidance state. Keep animation driven by achieved movement where possible so a blocked actor does not keep running at full stride.

**Check** a jump link, moving platform, and doorway entered while the actor is pushed. Inspect ownership changes, pose speed, and route recovery after returning to ordinary locomotion.

See [Asynchronous path requests](asynchronous-path-requests.md) for route request lifetimes and [Runtime navigation links](runtime-navigation-links.md) for authored traversal.
