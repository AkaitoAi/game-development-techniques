# Audio voice allocation

## Problem and design reasoning

A sound event can be valid while the mixer has no affordable playback slot. Frontier's *Planet Zoo*, *Planet Coaster*, *Jurassic World Evolution*, and *Elite Dangerous* required choosing among thousands of possible emitters while the camera moved freely. Matthew Florianz describes contextual mixing decisions that consider what listeners should perceive. This supports giving gameplay importance and audibility explicit roles in voice allocation. [Frontier, How Distance Has Shaped the Sound of Our Games, GDC 2021](https://www.gdcvault.com/play/1027544/how-distance-has-shaped-the)

The *Halo 5: Guardians* audio team also describes resolving performance challenges across engine integration, middleware, and content. The implementation recommendations below are inferred engineering applications of those constraints. A virtual voice can retain playback position without consuming a physical mixer voice, where the runtime supports that policy. [343 Industries, Managing Audio Performance, GDC 2016](https://gdcvault.com/play/1023229/Managing-Audio-Performance-Lessons-Learned)

## Techniques and practical checks

- Give each requested voice an owner, category, importance, estimated audibility, and retirement policy. Set per-category and global limits, with deterministic tie-breaking where reproducibility matters. **Check** a crowded scene with more requests than capacity. Verify that important warnings remain audible and low-priority loops yield predictably.
- Separate logical emitter lifetime from a physical playback slot. Decide whether a culled loop advances silently, restarts, or resumes at its retained position. **Check** move repeatedly beyond the audible range and back. Inspect phase continuity and verify the emitter never owns multiple physical voices after promotion.
- Apply hysteresis or a minimum hold time to prevent similarly ranked voices from repeatedly stealing each other's slots. Define short fades for safe transitions where appropriate. **Check** two equally important emitters near the threshold while the camera oscillates. Count steals and listen for clicks or repeated attacks.
- Cancel or transfer playback when an emitter is destroyed, pooled, or moves between scenes. Use a generation-aware handle for delayed completion callbacks. **Check** reuse an emitter immediately after stopping a long sound. Confirm an old callback cannot stop the new voice and that the voice count returns to baseline.

See [Audio](audio.md) for mix design and [Game object pooling](game-object-pooling.md) for object reuse contracts.
