# Entity component system design

## Problem and design reasoning

When many objects share rules but differ in their capabilities, a deep type hierarchy or scattered per-object updates can make both behavior and iteration costs hard to predict. Blizzard's Timothy Ford describes using an entity component system (ECS) to manage varied hero mechanics in the shipped Overwatch simulation. Stunlock's V Rising case study describes choosing ECS to support content scale and streaming. The archetype layout below is an application technique. The talks and case study establish the teams' goals. [Overwatch Gameplay Architecture and Netcode, GDC 2017](https://gdcvault.com/play/1024001/contactUs) · [Stunlock Studios, V Rising case study](https://unity.com/resources/stunlock-studios-v-rising)

## Techniques and verification

- Model capabilities with small components and give each gameplay field a clear writer. Schedule systems that read derived values after the authoritative writer. **Check** damage, healing, and death in one tick and record which system writes health and which emits the outcome.
- In an archetype ECS, group entities by component signature and plan queries around the sets systems actually read. Keep large cold payloads apart from frequently scanned data when profiling supports it. **Check** run representative mixes of active actors, static props, and sleeping actors. Compare query matches, frame time, and memory.
- Use component membership for changes that matter to query membership. For rapidly toggled state, consider a field or supported enable bit to avoid repeated migration. **Check** toggle status every tick at scale. Measure entity moves and check that readers observe the intended state in each phase.
- Define how systems handle entities with optional or incomplete capabilities at construction and teardown. **Check** stream a region out while an actor references a resident object, then load it again. Verify that references are resolved intentionally and dormant entities never enter an active-only query.
