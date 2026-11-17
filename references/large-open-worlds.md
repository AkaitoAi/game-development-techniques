# Large open worlds

## Problem and design reasoning

**Problem.** Expansion increases the work of laying out routes and placing scenery, and the densest districts exceed the runtime budget even when average density looks acceptable.

**Design reasoning.** Insomniac described movement routes and streaming regions separately, and Ubisoft kept natural pathways editable alongside procedural tooling. These choices suggest separating player traversal, content production, and runtime residency so each can be tuned for its own constraints.

A large open world needs manageable authoring units, routes that remain readable from several directions and budgets sized for its densest places. Treat design topology, authored ownership and runtime residency as separate representations that can share identifiers. For request scheduling and activation, see [World streaming](world-streaming.md).

## Shipped examples

**Sunset Overdrive (2014).** Insomniac documented the transition from linear levels to building routes around fast movement in an open city. Its level design presentation describes traversal markup and using the game's movement affordances to connect spaces. The accompanying engine postmortem explains how the city was split into hexagonal streaming regions. Designers can reason about movement routes while the runtime reasons about regional residency. [Liz England, Insomniac level design postmortem](https://media.gdcvault.com/gdc2015/presentations/England_Liz_TransitioningFromLinear.pdf) · [Elan Ruskin, streaming postmortem](https://media.gdcvault.com/gdc2015/presentations/Ruskin_Elan_SunsetOverdriveStreaming.pdf)

**Far Cry 5 (2018).** Ubisoft used procedural authoring tools for tasks including biome generation, freshwater networks, cliff rocks and terrain texturing. Ubisoft also describes the overall map as primarily hand-authored, with natural pathways developed in the context of player progression. The production technique is to automate repeated placement while keeping routes and authored intent editable. [Étienne Carrier, Ubisoft GDC presentation](https://www.gdcvault.com/play/1025557/Procedural-World-Generation-of-Far) · [Ubisoft developer interview](https://news.ubisoft.com/en-us/article/16TjVZmAtD85EWcvHtxHXL/far-cry-5-creating-curiosity-in-a-familiar-world)

**Halo Infinite (2021).** The terrain presentation describes a heightmap, tiled quadtree with resolution and update controls, virtual texturing and an offline sculpting graph. Artists could place spline road controls and use layered terrain volumes in a multi-user workflow. This demonstrates how runtime terrain budgets and the organization of editable terrain can evolve independently. [Mikael Nellfors, Building Zeta Halo](https://media.gdcvault.com/GDC%2B2022/Speaker%2BSlides/Building%2BZeta%2BHalo_Nellfors_Mikael.pdf)

## Techniques and practical checks

### Design coverage around traversal time

Sketch a graph of landmarks, routes, shortcuts and transitions. Measure travel using the production camera and movement modes. Put recognizable destinations or choices where sightlines reveal them, and give each route a reason to exist, such as a change in safety, travel time or access requirements. Extend the world only when there is a clear role for the new area.

**Check** with fresh players who approach objectives from different directions and with the slowest and fastest legal traversal modes. Record time between meaningful decisions, route reversals, lost players and duplicated encounter beats. Adjust travel time, visibility or connections before adding decorative density.

### Author regions with explicit seam contracts

Assign authored ownership for landmarks, road splines, waterways, quests and encounters. Let crossing features have one source owner and shared boundary data with stable IDs. Build local previews for each authoring region and a validation view that compares both sides of each boundary. Keep runtime package boundaries flexible when memory or streaming budgets require a different shape.

**Check** by editing one side of a seam. Rebuild a local region and then a wide-area build. Compare terrain and road height, slopes, collision, water flow, navigation links and duplicate identifiers across the seam. Repeat at each terrain detail level the player can traverse or see.

### Generate repeated detail with editable overrides

Derive candidate placement from authored inputs such as elevation, slope, proximity to paths and biome masks. Use seeds scoped to stable spatial keys so local edits can regenerate a small area. Expose exclusions and overrides around landmarks, key routes and encounters. Carry collision, navigation and memory costs into placement limits.

**Check** by regenerating twice, editing one biome mask and adding a route. Confirm that unaffected areas remain stable, roads remain passable, overrides survive, and local changes do not increase object density above the budget.

### Manage progression as world state

Make gates explicit in a progression graph. Store prerequisites and outcomes separately from the local scene objects that express them. Choose life-cycle rules for ambient actors, persistent actors and quest-critical actors. At distance, approximate simulation with coarse events when continuous behavior has no player-facing consequence.

**Check** by visiting objectives out of order, fast-traveling during an encounter, loading a checkpoint in another region and returning after background simulation has advanced. Inspect the gates and world state for stale objectives, duplicate rewards or unavailable routes. Profile an adverse scenario combining the densest view, many visible actors and rapid travel across a seam.
