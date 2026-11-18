# Gameplay spawn lifecycle programming

## Problem and design reasoning

A gameplay request can outlive its encounter, fail while queued, or yield an entity that later leaves its original owner. Tracking only live entity pointers can leave encounter counts, rewards, and network replicas inconsistent. *Assassin's Creed Unity* uses asynchronous spawn requests, executes urgent work each frame, and supports either constructing a new entity or acquiring a released one. Replicated state is applied before a replica enters the world, and crowd-station state tracks spawned, released, and lost NPCs. Bungie's *Destiny* talk emphasizes shared lifetime assumptions between engine systems. A portable inference is to give the gameplay request its own durable outcome and distinguish it from the entity's storage lifetime. [Charles Lefebvre, Networking Gameplay and AI in Assassin's Creed Unity, GDC 2015, slides 20–24 and 49](https://media.gdcvault.com/gdc2015/presentations/Lefebvre_Charles_NetworkingGameplayAndAI.pdf) · [Chris Butcher, Lessons from the Core Engine Architecture of Destiny, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Butcher_DestinyEngine_GDC2015_final.pdf)

## Techniques and practical checks

### Track requests independently of entity handles

Assign each logical spawn a request ID, owner generation, desired archetype, initial state, and terminal outcome. Treat queued, constructing, active, failed, and cancelled as explicit request states. When a repeated request means the same intended spawn, resolve it against that ID rather than creating another entity.

**Check** cancel an encounter while requests are queued, restart it, and deliver old completions afterward. Verify new encounter counts remain correct and every old request ends in one recorded terminal outcome.

### Register an active entity with its gameplay owner

Publish a ready entity and register its ownership in a controlled phase. Initialize encounter membership, network authority, save identity where applicable, and required subscriptions before ordinary gameplay ticks. Define how a partial registration unwinds.

**Check** inject failure between construction and registration, and between registration and activation. Verify no invisible entity contributes to objectives or population counts, and a retry produces one active instance.

### Distinguish death, release, and migration

Use explicit reasons when an entity leaves its owner. Death may complete an objective or grant a reward. Streaming removal, pool release, and ownership transfer may have different effects. Record the logical outcome before retiring the entity, and make repeat notifications resolve to the existing result.

**Check** kill a member, unload its region, transfer another member, and return a third to a pool. Deliver duplicate exit notifications and verify each reason affects population, objectives, and rewards according to its contract.

### Retire dependencies before reuse

Stop new actions and cancel owned work when a member exits. Unregister sensors, navigation requests, UI markers, and event listeners. Let delayed callbacks validate the retired generation. Preserve persistent state by logical identity when an instance is expected to reappear.

**Check** retire and reuse an entity while a path request, animation event, and reward callback are pending. Verify callbacks cannot mutate the replacement and reloading a persistent member restores the intended gameplay state.

See [Runtime prefab instantiation](runtime-prefab-instantiation.md) for construction and [Game object pooling](game-object-pooling.md) for reusable storage.
