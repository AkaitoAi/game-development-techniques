# Asynchronous path requests

## Problem and design reasoning

Pathfinding can outlive the decision that requested it. The target can move, the requesting actor can disappear, or a newer order can supersede the result while the search runs. In shipped *Horizon Zero Dawn*, Guerrilla redesigned navigation for varied character sizes and a dynamic open world. Its aerial planner progressively refined a route through runtime-generated navigation data. These cases motivate separating an actor's current intent from the work needed to find its route. The request protocol below is a portable engineering recommendation, rather than an attribution to Guerrilla's exact implementation. [Julian Berteling, Beyond Killzone, GDC 2018](https://gdcvault.com/play/1024912/Beyond-Killzone-Creating-New-AI) · [Guerrilla, Putting the AI Back Into Air](https://www.guerrilla-games.com/read/putting-the-ai-back-into-air)

Epic's asynchronous path API illustrates an explicit request identifier and completion delegate, including failure results. The engine-specific call is useful evidence for the contract a portable wrapper needs to expose. Check the project's engine version and callback-thread guarantees before implementing it. [Epic, FindPathAsync](https://dev.epicgames.com/documentation/unreal-engine/API/Runtime/NavigationSystem/UNavigationSystemV1/FindPathAsync?lang=en-US)

## Techniques and practical checks

### Bind results to the intent that created them

Give each actor a current request generation. Capture its stable handle, generation, destination, and movement capabilities with the request. On completion, accept only results that still match the live owner and intent. Use a short main-thread handoff if the navigation backend completes on a worker.

**Check** issue destination A and immediately replace it with B, then deliver A's result last. Destroy and respawn the actor in a reused slot. Verify that neither stale result takes control of the current actor.

### Bound the queue and coalesce redundant work

Keep only the latest equivalent request for an actor where gameplay permits. Prioritize work using interaction importance and request age, and keep starvation measurable. Preserve an explicit pending state so the actor has a defined behavior while waiting.

**Check** order a large group to move while a visible actor repeatedly changes goals. Record queue age, work per frame, and time to first usable path. Confirm low-priority actors eventually progress.

### Validate the route at publication and during use

Associate the result with relevant navigation revisions or dirty regions. Revalidate affected route segments when blockers or traversable surfaces change. When starting from a moved actor, reconnect to a legal nearby route segment or request a new route.

**Check** close a door after search begins, remove a destination surface, and move the actor before completion. Verify that publication and following each detect the invalid condition and use a bounded retry policy.

### Make every terminal outcome observable

Distinguish success, partial route, unreachable goal, cancellation, and owner loss. Release request storage in each case. Log the request identity with its goal and outcome so a stuck actor can be traced through request, completion, and following.

**Check** force every terminal outcome and repeat scene loads while requests are pending. Verify a completed or cancelled request cannot later emit a second gameplay completion and that queue storage returns to baseline.
