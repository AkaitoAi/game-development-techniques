# Collision filtering and layers

## Problem and design reasoning

Gameplay needs different meanings for a blocking contact, a trigger, a camera sweep, and a targeting ray. A single global collision switch cannot express those differences. Valve's released Source SDK for *Half-Life 2* and *Team Fortress 2* defines collision groups for debris, vehicles, players, projectiles, and scripted actors. It also applies game-rule and query-specific filters. In Sergiy Migdalskiy's GDC 2014 physics debugging presentation, an incorrect collision filter is a concrete example of a problem that needs inspection tools. A transferable design inference is to make filtering policy visible and testable at both simulation and query boundaries. [Valve collision group definitions](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/public/const.h) · [Valve physics contact filtering](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/physics.cpp) · [Sergiy Migdalskiy, Debugging Physics, GDC 2014](https://media.gdcvault.com/GDC2014/Presentations/Migdalskiy_Sergiy_Debugging_Physics.pdf)

## Techniques and practical checks

### Give simulation and queries explicit policies

Write a matrix for which categories generate blocking contacts, trigger overlaps, or neither. Define separate masks for camera, movement, interaction, and weapon queries where their purposes differ. Check the chosen engine's rules for pair filtering and query filtering, since bit masks alone do not specify all behavior. In Box2D, category/mask tests use both shapes, while a matching nonzero group index overrides those tests. [Box2D filtering documentation](https://box2d.org/documentation/md_simulation.html)

**Check** one fixture for every required matrix pair and each gameplay query. Confirm both the returned hits and the absence or presence of contact or trigger events, including a query starting inside an object.

### Model temporary exceptions by identity and lifetime

Use pair-specific exceptions when two related objects need temporary noncollision, such as an equipped item and its owner. Store the reason and owner of the exception, and release it through that lifetime's cleanup path. Use reference counts or tokens when multiple systems can request the same exception.

**Check** overlapping exception requests, unequipping, owner death, scene unload, and object pooling. Verify collision returns only after the final request ends and that a recycled object does not inherit the prior exception.

### Apply changes at a supported mutation point

Schedule filter changes through the engine's supported phase. Define what a newly enabled overlap means to gameplay and how old contacts are retired when filtering is disabled. Treat begin/end notifications as simulation results with possible latency, then make the gameplay response idempotent.

**Check** enable and disable a filter while bodies already overlap and while a query is queued. Record the change tick and contact events. Verify the result matches the intended policy and damage or pickup logic executes once.

### Expose filter decisions in diagnostics

Add a debug view that identifies shape category, query mask, pair exceptions, and the first policy rule that rejected a pair. Keep game-specific exceptions close to the system that owns them. Valve's *Team Fortress 2* movement code illustrates a query filter checking buildable ownership and bot roles. [Valve movement query filter](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/shared/tf/tf_gamemovement.cpp)

**Check** reproduce a deliberately missing collision and identify the rejecting rule from the diagnostics. Repeat in a packaged development build, then measure the filter's cost in a crowded fixture with many rejected pairs.
