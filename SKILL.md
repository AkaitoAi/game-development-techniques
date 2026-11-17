---
name: game-development-techniques
description: Apply techniques from shipped games to rendering, VFX, performance, physics, gameplay, controls, AI, world streaming, saves, audio, animation, procedural generation, quests, interface design, accessibility, and production tooling in any engine.
---

# Game development techniques

Read the relevant reference in this folder. If the agent does not support skill discovery, open this file directly and follow its relative links. For a Unity project, also read the applicable Unity workflow below and check the project's Editor and package versions.

| Open | When the task concerns |
|---|---|
| [Lighting](references/lighting.md) | Light and shadow, baked or dynamic lighting, readability, or lighting budgets |
| [Ambient occlusion](references/ambient-occlusion.md) | Contact shading, screen-space limits, and stability checks |
| [Light probes and irradiance](references/light-probes-and-irradiance.md) | Probe placement, indirect lighting interpolation, and leak checks |
| [Reflection rendering](references/reflection-rendering.md) | Screen-space reflections, probes, planar reflections, and fallbacks |
| [Water rendering](references/water-rendering.md) | Water surfaces, waves, reflections, and shoreline transitions |
| [Sky and weather](references/sky-and-weather.md) | Atmosphere, clouds, weather transitions, and visibility |
| [Shaders](references/shaders.md) | Material shading, vertex deformation, texture sampling, shader debugging, and variants |
| [Shader compilation and warmup](references/shader-compilation-and-warmup.md) | Pipeline creation, compilation hitches, warmup coverage, and cache validation |
| [Materials and textures](references/materials-and-textures.md) | Material inputs, texture import, compression, mipmaps, and asset validation |
| [Mobile texture formats](references/mobile-texture-formats.md) | Compressed texture support, import overrides, fallbacks, and residency |
| [Texture atlasing and batching](references/texture-atlasing-and-batching.md) | Atlas grouping, mip padding, render state, and draw cost |
| [Subsurface scattering](references/subsurface-scattering.md) | Light transport in skin and translucent materials, profiles, and performance |
| [Temporal rendering](references/temporal-rendering.md) | Frame history, motion vectors, temporal antialiasing, and upscaling |
| [HDR and post-processing](references/hdr-and-post-processing.md) | Exposure, tone mapping, color grading, and display output |
| [Color and visual readability](references/color-and-visual-readability.md) | Contrast, silhouettes, state cues, and color-independent recognition |
| [Rendering pipelines](references/rendering-pipelines.md) | Pass structure, buffers, visibility, and renderer architecture |
| [Render graph architecture](references/render-graph-architecture.md) | Pass dependencies, transient resources, barriers, and render scheduling |
| [GPU resource lifetimes](references/gpu-resource-lifetimes.md) | Completion fences, upload buffers, descriptor reuse, and retirement |
| [Mobile graphics scaling](references/mobile-graphics-scaling.md) | GPU tiers, dynamic quality, and visual quality floors |
| [Mobile shader budgets](references/mobile-shader-budgets.md) | Shader complexity, render passes, precision, and on-device profiling |
| [Mobile frame pacing](references/mobile-frame-pacing.md) | Refresh modes, frame intervals, missed deadlines, and presentation timing |
| [GPU-driven rendering](references/gpu-driven-rendering.md) | GPU visibility tests, draw compaction, indirect submission, and fallback paths |
| [Mesh LOD and impostors](references/mesh-lod-and-impostors.md) | Geometry simplification, screen-space transitions, and distant representations |
| [Visual effects](references/visual-effects.md) | Particles, effect events, overdraw budgets, and environmental reactions |
| [Navigation meshes and dynamic pathfinding](references/navigation-meshes-and-dynamic-pathfinding.md) | Walkable surfaces, moving blockers, traversal links, and replanning |
| [Animation events and gameplay timing](references/animation-events-and-gameplay-timing.md) | Action windows, event delivery, interruptions, and timing drift |
| [Baked visibility and portals](references/baked-visibility-and-portals.md) | Cell connectivity, visibility sets, occluders, and culling validation |
| [Shadow rendering](references/shadow-rendering.md) | Shadow maps, bias, filtering, caches, and artifact checks |
| [Network interest management](references/network-interest-management.md) | Replication relevance, subscription lifetimes, prioritization, and bandwidth |
| [Collectible and secret design](references/collectible-and-secret-design.md) | Discovery cues, tracking, optional rewards, and completion |
| [Decal rendering](references/decal-rendering.md) | Projection, surface filtering, layering, and lifetime |
| [Volumetric effects](references/volumetric-effects.md) | Fog volumes, scattering, temporal stability, and budgets |
| [Watercraft and buoyancy](references/watercraft-and-buoyancy.md) | Displacement, flotation, steering, and wave interaction |
| [Climbing and mantling](references/climbing-and-mantling.md) | Ledge detection, reach, traversal state, and collision clearance |
| [Level blockout and metrics](references/level-blockout-and-metrics.md) | Greyboxes, movement dimensions, sightlines, and iteration |
| [Animation state machines](references/animation-state-machines.md) | Transition rules, interruptions, state ownership, and debugging |
| [Ray tracing](references/ray-tracing.md) | Acceleration structures, ray budgets, denoising, and hybrid fallbacks |
| [Virtual texturing](references/virtual-texturing.md) | Tile residency, feedback, streaming priority, and fallback mips |
| [Player photography mechanics](references/player-photography-mechanics.md) | Subject recognition, shot scoring, and photography objectives |
| [Time manipulation](references/time-manipulation.md) | Rewind, time scaling, state restoration, and player-facing rules |
| [Motion capture pipeline](references/motion-capture-pipeline.md) | Capture planning, retargeting, cleanup, and motion validation |
| [Voice production and localization](references/voice-production-and-localization.md) | Dialogue recording, localized assets, timing, and pickup workflows |
| [Technical art](references/technical-art.md) | Art workflows, runtime constraints, and validation of authored assets |
| [Performance](references/performance.md) | Hitches, frame pacing, CPU or GPU budgets, culling, or streaming |
| [Mobile thermals and battery](references/mobile-thermal-and-battery.md) | Sustained device performance, thermal throttling, and power budgets |
| [Mobile memory pressure](references/mobile-memory-pressure.md) | Peak memory, pressure signals, asset eviction, and recovery |
| [Physics](references/physics.md) | Collision, simulation timing, character movement, or contact stability |
| [Physics query architecture](references/physics-query-architecture.md) | Query timing, collision filters, batch scheduling, and result validity |
| [Game math](references/game-math.md) | Vectors, coordinate transforms, rotations, interpolation, probability, and numerical precision |
| [Cross-platform determinism](references/cross-platform-determinism.md) | Reproducible simulation, stable ordering, numeric behavior, and desync diagnosis |
| [Deterministic random streams](references/deterministic-random-streams.md) | Seed ownership, independent random streams, replay, and diagnostics |
| [Destruction](references/destruction.md) | Breakable objects, fracture, debris, and gameplay state after damage |
| [Destructible terrain](references/destructible-terrain.md) | Editable terrain volumes, excavation, remeshing, collision, and persistence |
| [Game mechanics](references/game-mechanics.md) | Gameplay rules, resource loops, progression, or interactions between systems |
| [Design patterns for loops and challenge](references/design-patterns-loops-and-challenge.md) | Core loops, learning, mastery, difficulty, and pacing |
| [Design patterns for choice and progression](references/design-patterns-choice-and-progression.md) | Meaningful decisions, incentives, economies, and build diversity |
| [Design patterns for worlds and social play](references/design-patterns-worlds-and-social-play.md) | Exploration, narrative, cooperation, competition, and emergence |
| [Design patterns for information and sessions](references/design-patterns-information-and-sessions.md) | Player knowledge, feedback, commitments, recovery, and session boundaries |
| [Ability systems](references/ability-systems.md) | Activation rules, targeting, cooldowns, effects, and feedback |
| [Status effects](references/status-effects.md) | Buffs, debuffs, duration, stacking, dispels, and immunity |
| [Ability combos](references/ability-combos.md) | Setup and payoff, sequence rules, co-op attribution, and feedback |
| [Power-ups](references/power-ups.md) | Pickups, temporary forms, replacement rules, and level interactions |
| [Game modes and rule variants](references/game-modes-and-rule-variants.md) | Mode-specific rules, objectives, scoring, and match setup |
| [Sports game systems](references/sports-game-systems.md) | Sports rules, officiating, match simulation, and competitive fairness |
| [Cozy and social simulation](references/cozy-and-social-simulation.md) | Daily routines, relationships, optional goals, and low-pressure play |
| [Contextual interaction](references/contextual-interaction.md) | Interaction targets, action prompts, and interruptions |
| [Inventory and crafting](references/inventory-and-crafting.md) | Item stacks, equipment, recipe resolution, and atomic transfers |
| [Inventory UI and item comparison](references/inventory-ui-and-item-comparison.md) | Item details, comparison rules, sorting, and selection continuity |
| [Economy and progression](references/economy-and-progression.md) | Costs, rewards, resource flows, and pacing unlocks |
| [Loot generation and drop tables](references/loot-generation-and-drop-tables.md) | Weighted rewards, eligibility, duplicate policy, and distribution checks |
| [Skill trees and build diversity](references/skill-trees-and-build-diversity.md) | Unlock structure, meaningful choices, and respecialization |
| [Trading and player markets](references/trading-and-player-markets.md) | Trades, order books, settlement, and economy monitoring |
| [Game feel](references/game-feel.md) | Responsiveness, action timing, and visual, audio, or haptic feedback |
| [Combat design](references/combat-design.md) | Fictional combat timing, tactical choices, and attack readability |
| [Stealth design](references/stealth-design.md) | In-game detection rules, alert feedback, and recovery after discovery |
| [Turn-based tactics](references/turn-based-tactics.md) | Turn order, action budgets, previews, and resolving interactions |
| [Boss design](references/boss-design.md) | Encounter phases, readable patterns, challenge tuning, and retry pacing |
| [Cooperative gameplay](references/cooperative-gameplay.md) | Shared objectives, player roles, and coordination between teammates |
| [Haptics](references/haptics.md) | Tactile cues, device capabilities, effects mixing, and accessibility |
| [Onboarding and tutorials](references/onboarding-and-tutorials.md) | First sessions, teaching sequences, and measuring whether players learn |
| [Mobile first-session design](references/mobile-first-session-design.md) | Time to play, short teaching loops, interruption, and resuming a tutorial |
| [Mobile account onboarding](references/mobile-account-onboarding.md) | Guest identity, account linking, sign-in interruptions, and recovery |
| [Camera and controls](references/camera-and-controls.md) | Framing, tracking, occlusion, camera-relative movement, aim, and input mapping |
| [Camera composition and photo mode](references/camera-composition-and-photo-mode.md) | Shot composition, photo controls, and session state while composing |
| [Input rebinding](references/input-rebinding.md) | Binding capture, conflicts, persistence, prompts, and recovery |
| [Touch controls](references/touch-controls.md) | Gestures, touch target sizing, control ownership, and misinput recovery |
| [Mobile input latency](references/mobile-input-latency.md) | Touch sampling, gesture delay, input-to-display timing, and instrumentation |
| [Controller support and device handoff](references/controller-support-and-device-handoff.md) | Device ownership, hot-plugging, input transitions, and prompt updates |
| [VR interaction and comfort](references/vr-interaction-and-comfort.md) | Tracked input, locomotion comfort, spatial interaction, and head-relative feedback |
| [Mobile AR gameplay](references/mobile-ar-gameplay.md) | Camera tracking, placement, tracking recovery, and fallback play |
| [Player controllers](references/player-controllers.md) | Implementing player movement, traversal, grounding, or control handoff |
| [Vehicle controllers](references/vehicle-controllers.md) | Steering, handling, traction, and vehicle simulation |
| [Controller types](references/controller-types.md) | Choosing a movement or control model for a new feature |
| [Multiplayer movement](references/multiplayer-movement.md) | Movement authority, prediction, correction, and remote motion under latency |
| [Simulation snapshot interpolation](references/simulation-snapshot-interpolation.md) | Buffered snapshots, presentation time, discontinuities, and jitter handling |
| [Rollback in fighting games](references/rollback-fighting-games.md) | Predicted input, state restoration, resimulation, and visual correction |
| [Split-screen and local multiplayer](references/split-screen-and-local-multiplayer.md) | Viewport layout, local player ownership, joining, and shared performance |
| [Multiplayer matchmaking](references/multiplayer-matchmaking.md) | Queue policy, skill balance, network quality, and team assembly |
| [Cross-play and progression](references/cross-play-and-progression.md) | Platform interoperability, account linking, and progress across devices |
| [Multiplayer session lifecycle](references/multiplayer-session-lifecycle.md) | Joining, reconnecting, restoring session state, and teardown |
| [Replays](references/replays.md) | Recording, seeking, spectator views, highlights, and compatibility |
| [Spectators and tournaments](references/spectators-and-tournaments.md) | Live observers, competitive visibility, match operations, and broadcast controls |
| [Animation](references/animation.md) | Pose selection, transition timing, blending, locomotion, and motion alignment |
| [Animation compression](references/animation-compression.md) | Pose error, clip storage, decompression, and regression checks |
| [Animation runtime LOD](references/animation-runtime-lod.md) | Reducing pose evaluation cost, changing animation tiers, and preserving gameplay timing |
| [Animation rigging](references/animation-rigging.md) | Skeletons, skinning, deformation, and rig validation |
| [Cutscenes](references/cutscenes.md) | Cinematic sequencing, player control handoff, skipping, and continuity |
| [3D modeling](references/3d-modeling.md) | Mesh topology, UVs, rig compatibility, and game-ready asset budgets |
| [Character cloth and hair](references/character-cloth-and-hair.md) | Simulation, collisions, skinning fallback, and animation budgets |
| [Monetization](references/monetization.md) | Purchase UX, pricing clarity, entitlements, and transaction recovery |
| [Mobile rewarded ads](references/mobile-rewarded-ads.md) | Optional offers, completion handling, rewards, and player recovery |
| [Mobile in-app purchases](references/mobile-in-app-purchases.md) | Native store transactions, delayed delivery, restore, and reconciliation |
| [Procedural animation](references/procedural-animation.md) | Runtime pose generation, contact constraints, and physical response |
| [Level design](references/level-design.md) | Layout, wayfinding, encounter pacing, and teaching through space |
| [Level encounter scripting](references/level-encounter-scripting.md) | Triggers, encounter sequencing, cleanup, and interruption recovery |
| [Environmental storytelling](references/environmental-storytelling.md) | World details, evidence placement, and optional narrative discovery |
| [Maps and navigation aids](references/maps-and-navigation-aids.md) | Map readability, markers, route guidance, and discovery state |
| [Procedural generation](references/procedural-generation.md) | Seeds, constraints, generated content, and playability validation |
| [Procedural dungeons](references/procedural-dungeons.md) | Room graphs, generation constraints, progression gates, and solvability |
| [Quests and narrative](references/quests-and-narrative.md) | Quest state, branching, dialogue, and narrative continuity |
| [Quest failure and recovery](references/quest-failure-and-recovery.md) | Invalidated objectives, fallback branches, and recovery after interruption |
| [Systemic dialogue](references/systemic-dialogue.md) | Reactive lines, dialogue eligibility, arbitration, and repetition control |
| [Character customization](references/character-customization.md) | Appearance options, compatibility between parts, and customization persistence |
| [Composite character rendering](references/composite-character-rendering.md) | Modular character meshes, shared skeletons, seams, and rendering budgets |
| [Game AI](references/game-ai.md) | Navigation, behavior selection, perception, and encounter direction |
| [Game AI utility scoring](references/game-ai-utility-scoring.md) | Score curves, context inputs, tie-breaking, and decision stability |
| [Enemy spawn directors](references/enemy-spawn-directors.md) | Encounter pacing, spawn eligibility, and pressure budgets |
| [NPCs](references/npcs.md) | Civilian behaviors, routines, interaction, and contextual reactions |
| [Crowd simulation](references/crowd-simulation.md) | Dense populations, local movement, update tiers, and throughput |
| [Enemies](references/enemies.md) | Hostile roles, telegraphs, attack selection, and combat coordination |
| [Companions](references/companions.md) | Following, assisting, coordinating, and recovering separated allies |
| [Companion command interfaces](references/companion-command-interfaces.md) | Directing allies, command feedback, and execution failures |
| [World streaming](references/world-streaming.md) | Chunk loading, residency, unloading, and streaming performance |
| [Loading and transitions](references/loading-and-transitions.md) | Loading screens, progress feedback, and handoff into playable scenes |
| [Large open worlds](references/large-open-worlds.md) | World partitioning, content organization, budgets, and validation |
| [Coordinate origin rebasing](references/coordinate-origin-rebasing.md) | Large-world precision and continuity across origin shifts |
| [Mobile location-based play](references/mobile-location-based-play.md) | Location accuracy, map interactions, permissions, and safe play |
| [Terrain and vegetation](references/terrain-and-vegetation.md) | Landscape data, foliage placement, transitions, and rendering budgets |
| [Spline-based authoring](references/spline-based-authoring.md) | Roads, rails, rivers, curve sampling, and generated geometry |
| [Saves and persistent world state](references/save-systems.md) | Persistent identity, checkpointing, version migration, and recovery |
| [Checkpoints and respawn design](references/checkpoints-and-respawn-design.md) | Recovery placement, progress retention, and retry pacing |
| [Cloud save sync](references/cloud-save-sync.md) | Offline saves, multi-device conflicts, synchronization, and recovery |
| [Audio](references/audio.md) | Spatial cues, adaptive music, dynamic mixing, and sound as gameplay feedback |
| [Mobile audio routing](references/mobile-audio-routing.md) | Device routes, audio focus, Bluetooth, and interruption recovery |
| [Spatial audio and acoustics](references/spatial-audio-and-acoustics.md) | Listener-relative sound, occlusion, propagation, and room transitions |
| [Adaptive music](references/adaptive-music.md) | Musical transitions, layers, gameplay state, and timing verification |
| [User interface](references/user-interface.md) | HUDs, menus, input focus, and readable gameplay information |
| [Mobile safe areas and orientation](references/mobile-safe-areas-and-orientation.md) | Cutouts, rotation, display insets, and responsive gameplay layout |
| [Diegetic interfaces](references/diegetic-interfaces.md) | World-space information, readability, interaction, and accessible alternatives |
| [Settings and presets](references/settings-and-presets.md) | Defaults, overrides, device capabilities, and settings recovery |
| [Localization](references/localization.md) | Translated content, text layout, assets, and language QA |
| [Mobile localization](references/mobile-localization.md) | App language preferences, small-screen text, localized downloads, and mobile store checks |
| [Accessibility](references/accessibility.md) | Alternative inputs, sensory cues, assist settings, and accessible playtesting |
| [Mobile accessibility and one-handed play](references/mobile-accessibility-and-one-handed-play.md) | Reachability, alternatives to gestures, assist settings, and device testing |
| [Production and tooling](references/production-and-tooling.md) | Editors, authoring workflows, asset builds, iteration, and validation |
| [Game design documentation](references/game-design-documentation.md) | Design intent, rules documentation, decisions, and handoff |
| [Build automation](references/build-automation.md) | Repeatable builds, pipeline dependencies, caching, and artifact verification |
| [Game prototyping](references/game-prototyping.md) | Testing design assumptions, judging prototype results, and planning a vertical slice |
| [Gameplay architecture](references/gameplay-architecture.md) | Entity composition, gameplay state ownership, and object lifecycles |
| [Programming patterns](references/programming-patterns.md) | State, commands, composition, and choosing where patterns simplify code |
| [Gameplay state machines](references/gameplay-state-machines.md) | State ownership, transition guards, interruptions, and recovery |
| [Engine subsystem lifecycles](references/engine-subsystem-lifecycles.md) | Startup dependencies, scene and session ownership, teardown, and recovery |
| [Game module dependencies](references/game-module-dependencies.md) | Module boundaries, dependency direction, and build-time coupling |
| [Game plugin interfaces](references/game-plugin-interfaces.md) | Extension contracts, capability discovery, compatibility, and isolation |
| [Gameplay ownership and authority](references/gameplay-ownership-and-authority.md) | State ownership, mutation rights, authority transfer, and conflict handling |
| [Game object pooling](references/game-object-pooling.md) | Reusable objects, reset contracts, capacity, and stale callbacks |
| [Simulation record-and-replay testing](references/simulation-record-and-replay-testing.md) | Capturing regression traces, replaying inputs, and locating divergence |
| [Gameplay hot reload](references/gameplay-hot-reload.md) | Reloading gameplay data or code, preserving state, and handling failures |
| [Async asset lifetimes](references/async-asset-lifetimes.md) | Load requests, cancellation, ownership, shared dependencies, and release |
| [Asynchronous task cancellation](references/asynchronous-task-cancellation.md) | Scoped work, cancellation propagation, completion races, and teardown |
| [Game asset dependency graphs](references/game-asset-dependency-graphs.md) | Dependency discovery, stable IDs, invalidation, and bundle validation |
| [Derived asset build caches](references/derived-asset-build-caches.md) | Cache keys, invalidation, artifact consistency, and clean-build checks |
| [Gameplay serialization and snapshots](references/gameplay-serialization-and-snapshots.md) | Runtime state capture, restoration, identity, and replay checks |
| [Game engine job scheduling](references/game-engine-job-scheduling.md) | Dependencies, task granularity, critical paths, and frame budgets |
| [Animation job pipelines](references/game-animation-job-pipelines.md) | Parallel pose evaluation, dependency barriers, and presentation handoff |
| [Gameplay invariants and validation](references/gameplay-invariants-and-validation.md) | Rules that must hold across transitions, authoring checks, and runtime diagnostics |
| [Gameplay tags and capabilities](references/gameplay-tags-and-capabilities.md) | Shared vocabulary, eligibility queries, grants, and state cleanup |
| [Gameplay transaction boundaries](references/gameplay-transaction-boundaries.md) | Atomic state changes, commit points, retries, and recovery |
| [Game content validation pipelines](references/game-content-validation-pipelines.md) | Asset rules, authoring feedback, and build validation |
| [Game thread ownership](references/game-thread-ownership.md) | Single-writer state, handoff, synchronization, and teardown |
| [Game runtime feature flags](references/game-runtime-feature-flags.md) | Runtime configuration contracts, coordinated activation, and recovery |
| [Save schema migrations](references/save-schema-migrations.md) | Versioned formats, content identifiers, migration chains, and recovery |
| [Entity identifiers and handles](references/entity-identifiers-and-handles.md) | Stale references, recycled slots, persistent IDs, and deferred resolution |
| [Gameplay scripting boundaries](references/gameplay-scripting-boundaries.md) | Script/native contracts, state ownership, reloads, and error handling |
| [Fixed timestep and update loops](references/fixed-timestep-and-update-loops.md) | Simulation steps, interpolation, scheduling, and hitch behavior |
| [Game clock domains](references/game-clock-domains.md) | Simulation, presentation, and wall clocks, pause rules, and timer ownership |
| [Runtime debugging tools](references/runtime-debugging-tools.md) | Inspectors, overlays, traces, and reproducible gameplay diagnostics |
| [Data-oriented architecture](references/data-oriented-architecture.md) | Data layout, batched updates, access patterns, and profiling |
| [Data layout and cache locality](references/data-layout-and-cache-locality.md) | Hot and cold data, traversal order, memory traffic, and profiling |
| [Entity-component-system design](references/entity-component-system-design.md) | Component ownership, queries, archetypes, and structural changes |
| [Simulation command buffers](references/simulation-command-buffers.md) | Deferred mutations, playback barriers, conflict resolution, and ordering |
| [Spatial indexing](references/spatial-indexing.md) | Spatial queries, moving objects, partitioning, and correctness under load |
| [Game engine memory allocators](references/game-engine-memory-allocators.md) | Arenas, alignment, lifetime classes, and fragmentation |
| [Gameplay messaging](references/gameplay-messaging.md) | Events, commands, ordering, subscriber lifetimes, and debugging |
| [Resimulation-safe gameplay events](references/resimulation-safe-gameplay-events.md) | Replayable events, confirmed side effects, and feedback deduplication |
| [Network message schema evolution](references/network-message-schema-evolution.md) | Protocol compatibility, message versions, and mixed-build validation |
| [Data-driven gameplay](references/data-driven-gameplay.md) | Authored definitions, validation, versioning, and runtime state boundaries |
| [Memory management](references/memory-management.md) | Allocation, pooling, lifetimes, memory ceilings, and leaks |
| [Game runtime memory budgets](references/game-runtime-memory-budgets.md) | Peak residency, budget ownership, streaming headroom, and pressure responses |
| [Multithreading and jobs](references/multithreading-and-jobs.md) | Parallel work, dependencies, synchronization, and profiling |
| [2D rendering](references/2d-rendering.md) | Sprites, atlases, tilemaps, scaling, and pixel stability |
| [Puzzle design](references/puzzle-design.md) | Constraints, solution discovery, hints, and solvability |
| [Difficulty and balancing](references/difficulty-and-balancing.md) | Challenge curves, assists, balance changes, and measuring outcomes |
| [Roguelike run design](references/roguelike-run-design.md) | Run structure, draft choices, escalation, and progression between runs |
| [Platforming challenges](references/platforming-challenges.md) | Jump metrics, obstacle sequences, moving platforms, and checkpoints |
| [Racing track design](references/racing-track-design.md) | Corner sequencing, alternate lines, overtaking, and track validation |
| [Survival and base building](references/survival-and-base-building.md) | Resource pressure, construction, structural rules, and recovery |
| [Strategy and management simulation](references/strategy-and-management-simulation.md) | Production chains, bottlenecks, simulation clocks, and feedback |
| [Deckbuilding and card rules](references/deckbuilding-and-card-rules.md) | Draw and discard systems, card effects, resolution order, and deck strategy |
| [Rhythm games](references/rhythm-games.md) | Musical timing, input calibration, chart authoring, and scoring |
| [Asynchronous multiplayer](references/asynchronous-multiplayer.md) | Turn submission, delayed interaction, deadlines, and resuming shared games |
| [Player communications](references/player-communications.md) | Voice, text, pings, accessibility, privacy, and player controls |
| [Asset source control](references/asset-source-control.md) | Versioning large assets, ownership, locking, and reproducible revisions |
| [Downloadable content](references/downloadable-content.md) | Optional content packaging, entitlements, installation, and compatibility |
| [Horror design](references/horror-design.md) | Fictional tension, uncertainty, pacing, and sensory cues |
| [Playtesting](references/playtesting.md) | Observing players, designing sessions, and validating design decisions |
| [Release engineering](references/release-engineering.md) | Build promotion, patch delivery, version skew, and recovery |
| [Mobile store delivery](references/mobile-store-delivery.md) | Store builds, asset packs, updates, and install verification |
| [Mobile downloads and data budgets](references/mobile-downloads-and-data-budgets.md) | In-game downloads, metered networks, resumability, and storage |
| [Mobile device storage](references/mobile-device-storage.md) | Disk budgets, storage eviction, offline assets, and low-space recovery |
| [Game demos and trials](references/game-demos-and-trials.md) | Scope, first-session goals, save transfer, and conversion handoff |
| [Build size and installation](references/build-size-and-installation.md) | Package size, optional content, installation order, and time to playable |
| [Launch and certification](references/launch-and-certification.md) | Submission builds, platform requirements, release readiness, and launch support |
| [Game preservation](references/game-preservation.md) | Archiving releases, preserving build inputs, and validating long-term playability |
| [Live operations](references/live-operations.md) | Live events, feature schedules, operational monitoring, and event recovery |
| [Mobile soft launch](references/mobile-soft-launch.md) | Limited rollout, cohort measurement, launch gates, and expansion |
| [Mobile push notifications](references/mobile-push-notifications.md) | Opt-in, timely messages, deep links, and notification preferences |
| [Backend reliability](references/backend-reliability.md) | Capacity, service degradation, retries, and incident recovery |
| [Mobile offline play and networking](references/mobile-offline-and-networking.md) | Connection changes, offline progress, synchronization, and retry behavior |
| [Dedicated server operations](references/dedicated-server-operations.md) | Server allocation, fleet capacity, deployments, and draining sessions |
| [Community moderation](references/community-moderation.md) | Player reporting, review, enforcement, appeals, and safety controls |
| [Platform porting](references/platform-porting.md) | Platform adapters, packaged builds, capability tiers, and parity checks |
| [Mobile lifecycle and interruptions](references/mobile-lifecycle-and-interruptions.md) | Backgrounding, suspension, interruption, and state restoration |
| [User-generated content](references/user-generated-content.md) | Player creation tools, publishing, versioning, and playable content validation |
| [Game mod support](references/game-mod-support.md) | Mod interfaces, content compatibility, load order, and recovery |
| [In-game editor design](references/in-game-editor-design.md) | Authoring controls, undo, snapping, validation, and playtest loops |
| [Game testing](references/game-testing.md) | Regression fixtures, soak tests, automation, and build verification |
| [Mobile device coverage](references/mobile-device-coverage.md) | Device selection, OS and GPU diversity, and release testing |
| [Gameplay telemetry](references/gameplay-telemetry.md) | Event design, instrumentation, and interpreting player behavior data |
| [Crash diagnostics](references/crash-diagnostics.md) | Crash capture, symbolication, grouping, and regression verification |
| [Achievements and leaderboards](references/achievements-and-leaderboards.md) | Unlock conditions, score submission, ranking, and season boundaries |

1. Identify the player-visible behavior and the target conditions. For a regression, capture a baseline using a reproducible scene, input sequence, or playtest scenario.
2. Start with **Problem and design reasoning** in the relevant guide. Match the observed symptom and project constraints to the problem the technique addresses. Treat documented developer rationale as evidence about that game and labeled inferences as hypotheses to test. Map the chosen technique to the project's update loop, asset pipeline, or rules, and tune it using measurements from the target game.
3. Test at the normal entry point with a repeatable comparison. Use frame-time traces for performance, input and state traces for simulation and control timing, a representative playtest for experiential changes, and clean and incremental builds for pipeline changes. Record the conditions and the observed result.

If symptoms overlap, start with the closest measurable cause. For a late jump, trace input arrival, eligibility, movement, and feedback timestamps before choosing between the physics and game-feel references.

## Unity workflows

| Open | When the task concerns |
|---|---|
| [Unity architecture](references/unity-architecture.md) | ScriptableObjects, event channels, object pooling, and prefab variants |
| [Unity rendering](references/unity-rendering.md) | URP pass ordering, shader variants, VFX Graph, and overdraw |
| [Unity performance](references/unity-performance.md) | Target-device profiling, import settings, quality tiers, and memory |
| [Unity UI and math](references/unity-ui-and-math.md) | UI Toolkit or uGUI, responsive layouts, debug vectors, and rotation spaces |

The [Unity book index](references/unity-books.md) identifies the supplied PDFs behind these workflows and records the relevant pages.
