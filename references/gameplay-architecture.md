# Gameplay architecture and entity lifecycles

## Problem and design reasoning

**Problem.** Per-object scripts disagree about who owns a piece of state, or many otherwise flexible objects become expensive just by being active.

**Design reasoning.** Massive describes moving frequently executed behavior into shared systems while using graphs to configure it. That design keeps the useful authoring choices accessible and gives the runtime one place to control execution cost.

## Shipped examples

**Doom (1993).** The released Linux source stores active updates in a thinker list. A moving door and a moving actor can both register their own update callbacks. Removal marks a thinker for cleanup as the list is processed. This gives distinct game objects a shared lifetime and scheduling convention. [id Software, thinker loop](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/p_tick.c) · [Door code](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/p_doors.c)

**Overwatch (2016).** Blizzard used an entity-component-system architecture for gameplay. Timothy Ford's talk explains how the simulation's architecture supports responsiveness and precision in a networked game. Component state and simulation systems provide explicit boundaries for authors to reason about. [Timothy Ford, GDC 2017](https://gdcvault.com/play/1024001/-Overwatch-Gameplay-Architecture-and)

**Avatar: Frontiers of Pandora (2023).** Massive described moving behavior from frequently executed per-object graphs into gameplay systems, using other graphs to configure behavior, and analyzing graphs for cheaper paths. The recommendation follows from the cost of running flexible scripts for many active objects. [Joshua Simmons, GDC 2024, slides on gameplay graphs](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/Simmons_Joshua_Upgrading_the_Snowdrop.pdf)

## Techniques and practical checks

### Separate capability from identity

Give an entity a stable handle and approach its abilities as composable state. Start with the smallest practical distinction, such as movement, health, and interactability. Make the owning system explicit for each field. Introduce more specialized scheduling or storage when a measured hotspot calls for it.

**Check.** Add movement to an object that previously stayed still, or reuse interaction on a new object type. Confirm that neither change requires modifying unrelated object families and that the inspector exposes the source of each capability.

### Define lifecycle transitions and ownership

Distinguish creation, registration, activation, deactivation, and destruction where subsystems need those transitions. Decide which scene, session, or persistent domain owns the entity. Use handles with generation checks if indexes can be reused. Complete teardown before recycling the slot.

**Check.** Repeatedly spawn and despawn an object while a delayed effect refers to its handle. Reload the owning scene and reuse the same slot. Confirm listeners detach and delayed work cannot mutate the replacement.

### Apply structural changes at a named boundary

Specify when systems observe created, removed, and modified entities. If direct mutation would invalidate iteration, collect structural commands for a defined application phase. Define which requests win and what happens when an entity is created and destroyed before other systems see it.

**Check.** Destroy the current entity and request another spawn from the same update. Verify each intended update runs once, and rerun a trace with competing changes to confirm the documented ordering.

### Keep simulation decisions inspectable

Give consequential events a source, target, and tick or sequence identifier. Feed presentation from outcomes. Record enough authoritative state and input to reproduce state transitions in a small test scene.

**Check.** Run the same scenario with no camera or effects, then in the full game. Compare entity state at the same simulation tick. Inspect a duplicate or missing outcome by following its sequence through systems and listeners.
