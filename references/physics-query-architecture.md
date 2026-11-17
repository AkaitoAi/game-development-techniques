# Physics query architecture

## Problem and design reasoning

Gameplay, camera, and audio systems can all ask the physics world a question while needing different shapes, filters, and timing guarantees. In *Half-Life 2*, Valve's released player source includes both a hull trace and a line trace with explicit content masks and self-exclusion in a movement-related probe. That combination separates clearance of a volume from a line test toward the floor. [Valve, Source SDK player code](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/hl2/hl2_player.cpp)

PlatinumGames' Shuji Kohata describes sampling the environment with multiple rays across frames in *NieR:Automata* to drive interactive reverb and filtering. An inference from these examples is to budget and schedule collision questions by the latency their consumers can tolerate. A movement decision may need current results while an ambient estimate can accumulate evidence over several frames. [Shuji Kohata, An Interactive Sound Dystopia, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Kohata_Shuji_An_Interactive_Sound.pdf)

## Techniques

- Choose a ray for a line, a sweep for travel of a body, and an overlap for occupancy. Define the semantics of touching, starting inside geometry, and zero-length movement. **Check** fixtures for thin obstacles, exact boundary contact, initial overlap, and zero-length inputs. Compare each result against its documented contract.
- Create named filter policies for distinct consumers. Specify world and dynamic geometry, triggers, ignored objects, and what produces a blocking hit. **Check** one fixture containing the querying actor, a trigger, solid geometry, and a moving object. Verify each query profile returns only its intended hits.
- Define which simulation phase a query observes. Batch low-urgency queries against a synchronized snapshot where supported, and tag results with the sampled tick and object generation. **Check** move or remove a target while a batch is queued. Verify that stale results are discarded or applied only within the consumer's permitted tolerance.
- Set workload limits and make the result policy explicit. Use any-hit when identity and distance do not matter, nearest-hit when they do, and bounded collections for multi-hit consumers. **Check** a dense scene with coincident hits and more candidates than the buffer can hold. Confirm overflow behavior, stable tie-breaking where required, and a measured query budget.
