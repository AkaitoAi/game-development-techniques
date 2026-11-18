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
| [Particle system architecture](references/particle-system-architecture.md) | Emitters, simulation, pooling, rendering, and event lifetimes |
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
| [Physics constraints and joints](references/physics-constraints-and-joints.md) | Constraint frames, limits, motors, stability, and solver budgets |
| [Collision filtering and layers](references/collision-filtering-and-layers.md) | Collision matrices, query masks, trigger policy, and runtime changes |
| [Physics query architecture](references/physics-query-architecture.md) | Query timing, collision filters, batch scheduling, and result validity |
| [Physics substep events](references/game-engine-substep-events.md) | Bridging physics callbacks to gameplay, contact lifetimes, and event deduplication |
| [Game math](references/game-math.md) | Vectors, coordinate transforms, rotations, interpolation, probability, and numerical precision |
| [Cross-platform determinism](references/cross-platform-determinism.md) | Reproducible simulation, stable ordering, numeric behavior, and desync diagnosis |
| [Deterministic random streams](references/deterministic-random-streams.md) | Seed ownership, independent random streams, replay, and diagnostics |
| [Destruction](references/destruction.md) | Breakable objects, fracture, debris, and gameplay state after damage |
| [Destructible terrain](references/destructible-terrain.md) | Editable terrain volumes, excavation, remeshing, collision, and persistence |
| [Game mechanics](references/game-mechanics.md) | Gameplay rules, resource loops, progression, or interactions between systems |
| [Traversal route design](references/traversal-route-design.md) | Approach, movement sequences, commitment, and recovery paths |
| [Game asset wear authoring](references/game-asset-wear-authoring.md) | Material history, wear placement, scale, and readability |
| [UI empty and loading states](references/ui-empty-and-loading-states.md) | Absent content, pending data, recovery, and interaction state |
| [Sound loop design](references/sound-loop-design.md) | Loop boundaries, variation, transitions, and repetition |
| [Runtime audio zone programming](references/runtime-audio-zone-programming.md) | Zone occupancy, blending, listener changes, and teardown |
| [Mesh vertex color workflows](references/mesh-vertex-color-workflows.md) | Attribute contracts, painting, interpolation, and import validation |
| [Enemy telegraph design](references/enemy-telegraph-design.md) | Warning timing, threat recognition, response opportunities, and cue overlap |
| [Mesh baking workflows](references/mesh-baking-workflows.md) | High and low meshes, projection cages, shading bases, and bake checks |
| [UI notification priority](references/ui-notification-priority.md) | Notification arbitration, lifetime, grouping, and interruption |
| [Impact sound design](references/impact-sound-design.md) | Material pairs, contact strength, outcome cues, and mix clarity |
| [Runtime surface classification](references/runtime-surface-classification.md) | Surface identity, query results, fallbacks, and consumer consistency |
| [Mesh pivot authoring](references/mesh-pivot-authoring.md) | Placement anchors, rotational behavior, variants, and import checks |
| [Boss arena design](references/boss-arena-design.md) | Spatial affordances, attack coverage, recovery, and arena state changes |
| [UV layout authoring](references/uv-layout-authoring.md) | Island planning, seams, density, padding, and import checks |
| [UI tutorial overlays](references/ui-tutorial-overlays.md) | Context triggers, input ownership, interruption, and tutorial persistence |
| [Sound distance design](references/sound-distance-design.md) | Audible ranges, distance cues, prioritization, and listener perspective |
| [Runtime object selection programming](references/runtime-object-selection-programming.md) | Picking queries, selection identity, input arbitration, and invalidation |
| [Game mesh import validation](references/game-mesh-import-validation.md) | Import contracts, transforms, attributes, topology, and repeatable builds |
| [Player goal signposting](references/player-goal-signposting.md) | Goal transitions, attention cues, confirmation, and recovery from uncertainty |
| [Environment prop storytelling](references/environment-prop-storytelling.md) | Prop relationships, evidence, composition, and narrative interpretation |
| [UI icon system design](references/ui-icon-system-design.md) | Symbol vocabulary, ambiguity, consistency, and recognition checks |
| [Sound variation authoring](references/sound-variation-authoring.md) | Variation sets, repetition limits, parameter ranges, and contextual coherence |
| [Runtime object highlight programming](references/runtime-object-highlight-programming.md) | Highlight ownership, rendering state, priorities, and cleanup |
| [Collision proxy authoring](references/collision-proxy-authoring.md) | Simplified shapes, physical intent, import contracts, and contact checks |
| [Encounter recovery design](references/encounter-recovery-design.md) | Regaining control, safe recovery windows, and encounter re-entry |
| [Trim sheet authoring](references/trim-sheet-authoring.md) | Reusable surface strips, texel density, UV alignment, and seams |
| [UI feedback and confirmation](references/ui-feedback-and-confirmation.md) | Action acknowledgment, pending states, confirmation cost, and outcome clarity |
| [Dialogue mix priority](references/dialogue-mix-priority.md) | Speech importance, competing lines, ducking, and intelligibility |
| [Procedural look-at programming](references/procedural-look-at-programming.md) | Target arbitration, pose constraints, smoothing, and ownership |
| [Mesh normal and tangent validation](references/mesh-normal-and-tangent-validation.md) | Shading bases, mirrored UVs, import conventions, and deformation checks |
| [Reward reveal design](references/reward-reveal-design.md) | Outcome presentation, pacing, acknowledgment, and recovery |
| [Character topology for deformation](references/character-topology-for-deformation.md) | Edge flow, joint deformation, silhouette, and pose checks |
| [UI information hierarchy](references/ui-information-hierarchy.md) | Priority, grouping, progressive disclosure, and information density |
| [Interactive music stingers](references/interactive-music-stingers.md) | Event accents, musical timing, arbitration, and repetition |
| [Runtime socket validation](references/runtime-socket-validation.md) | Socket contracts, rig compatibility, missing anchors, and diagnostics |
| [Asset LOD authoring](references/asset-lod-authoring.md) | Simplification targets, material consistency, transition checks, and import validation |
| [Encounter role composition](references/encounter-role-composition.md) | Enemy roles, complementary pressures, readable combinations, and pacing |
| [Hard-surface game assets](references/hard-surface-game-assets.md) | Shape hierarchy, normals, bevels, UVs, and in-game asset checks |
| [UI error and recovery design](references/ui-error-and-recovery-design.md) | Actionable failures, progress preservation, retries, and recovery paths |
| [Soundscape density design](references/soundscape-density-design.md) | Ambient layers, perceptual space, cue priority, and scene transitions |
| [Runtime equipment attachment programming](references/runtime-equipment-attachment-programming.md) | Sockets, item ownership, attachment updates, and state changes |
| [Game material authoring validation](references/game-material-authoring-validation.md) | Material input contracts, reference lighting, asset checks, and platform variants |
| [Difficulty communication](references/difficulty-communication.md) | Challenge expectations, readable failure, and assist feedback |
| [Modular asset kit design](references/modular-asset-kit-design.md) | Reusable geometry, grid rules, sockets, and seams |
| [UI transition design](references/ui-transition-design.md) | Navigation continuity, motion timing, input handoff, and interruption |
| [Interactive foley systems](references/interactive-foley-systems.md) | Motion-driven sound, surfaces, variation, and mix priorities |
| [Runtime footstep programming](references/runtime-footstep-programming.md) | Contact events, surface queries, timing, and duplicate prevention |
| [Game asset scale conventions](references/game-asset-scale-conventions.md) | Units, import transforms, pivots, and cross-discipline validation |
| [Design patterns for loops and challenge](references/design-patterns-loops-and-challenge.md) | Core loops, learning, mastery, difficulty, and pacing |
| [Design patterns for choice and progression](references/design-patterns-choice-and-progression.md) | Meaningful decisions, incentives, economies, and build diversity |
| [Design patterns for worlds and social play](references/design-patterns-worlds-and-social-play.md) | Exploration, narrative, cooperation, competition, and emergence |
| [Design patterns for information and sessions](references/design-patterns-information-and-sessions.md) | Player knowledge, feedback, commitments, recovery, and session boundaries |
| [Ability systems](references/ability-systems.md) | Activation rules, targeting, cooldowns, effects, and feedback |
| [Gameplay ability prediction](references/gameplay-ability-prediction.md) | Responsive activation, authoritative confirmation, and correction |
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
| [Tween systems](references/tween-systems.md) | Interpolation, easing, cancellation, sequencing, and target ownership |
| [Ragdoll systems](references/ragdoll-systems.md) | Animation and physics transitions, constraints, recovery, and network state |
| [Inverse kinematics](references/inverse-kinematics.md) | Solver chains, targets, constraints, update order, and pose validation |
| [Motion warping and alignment](references/motion-warping-and-alignment.md) | Adapting authored movement to targets, collision, and interruption |
| [Animation layer composition](references/animation-layer-composition.md) | Pose masks, additive layers, evaluation order, and ownership |
| [Animation root motion](references/game-animation-root-motion.md) | Authored displacement, collision, root alignment, and movement ownership |
| [Game transform hierarchies](references/game-transform-hierarchies.md) | Local and world transforms, parenting, propagation, and attachments |
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
| [Runtime save slot management](references/runtime-save-slot-management.md) | Slot identity, concurrent requests, profile ownership, and recovery |
| [Game checksum programming](references/game-checksum-programming.md) | Canonical state, hash boundaries, deterministic ordering, and divergence checks |
| [Animation compression streaming](references/runtime-animation-compression-streaming.md) | Compressed blocks, pose sampling, residency, and latency |
| [Game runtime binary formats](references/game-runtime-binary-formats.md) | Versioned assets, offsets, validation, compatibility, and loading |
| [Runtime animation notify queues](references/runtime-animation-notify-queues.md) | Temporal event capture, queue publication, interruption, and deduplication |
| [Animation curve programming](references/animation-curve-programming.md) | Sampling, blending, value ownership, discontinuities, and precision |
| [Gameplay action cancellation](references/gameplay-action-cancellation.md) | Multi-phase actions, commit points, interruption, and resource release |
| [Gameplay command validation](references/gameplay-command-validation.md) | Command identity, eligibility, state revisions, and rejection behavior |
| [Runtime decal programming](references/runtime-decal-programming.md) | Placement, surface attachment, pooling, and retirement |
| [Particle event programming](references/particle-event-programming.md) | Simulation events, CPU and GPU delivery, bounds, and callback ownership |
| [Gameplay save dirty tracking](references/gameplay-save-dirty-tracking.md) | Dirty revisions, snapshot capture, completion, and repeat writes |
| [Runtime property change tracking](references/runtime-property-change-tracking.md) | Mutation ownership, revision counters, observers, and change coalescing |
| [Physics query result lifetimes](references/physics-query-result-lifetimes.md) | Result snapshots, object generations, delayed consumption, and buffer reuse |
| [Runtime bounds maintenance](references/runtime-bounds-maintenance.md) | Dirty bounds, transforms, deformation, and conservative visibility |
| [Network entity spawn ordering](references/network-entity-spawn-ordering.md) | Replica readiness, dependency references, reordering, and retirement |
| [Gameplay cooldown programming](references/gameplay-cooldown-programming.md) | Charge state, deadlines, modifiers, pause behavior, and correction |
| [Networked animation state](references/networked-animation-state.md) | Replicated action state, pose timing, correction, and presentation |
| [Animation transition interruption](references/animation-transition-interruption.md) | Transition ownership, interruption rules, blend continuity, and event cleanup |
| [Gameplay coroutines and tasks](references/gameplay-coroutines-and-tasks.md) | Continuation lifetimes, waits, cancellation, and execution scope |
| [Runtime main thread dispatch](references/runtime-main-thread-dispatch.md) | Completion queues, safe phases, stale results, and overload |
| [World streaming reference management](references/world-streaming-reference-management.md) | Cross-cell references, load ownership, proxies, and safe unloading |
| [Runtime navigation mesh updates](references/runtime-navigation-mesh-updates.md) | Dirty tiles, rebuild jobs, publication, and path invalidation |
| [Procedural generation job pipelines](references/procedural-generation-job-pipelines.md) | Deterministic stages, dependencies, cancellation, and chunk publication |
| [Gameplay spawn lifecycle programming](references/gameplay-spawn-lifecycle-programming.md) | Spawn requests, initialization, authority, activation, and retirement |
| [Save and load threading](references/save-load-threading.md) | Coherent capture, background serialization, restore ownership, and cancellation |
| [Runtime scene transition programming](references/runtime-scene-transition-programming.md) | Transition state, activation barriers, cancellation, and recovery |
| [Animation pose caching](references/animation-pose-caching.md) | Cache keys, pose ownership, invalidation, and evaluation cost |
| [Runtime event subscription lifetimes](references/runtime-event-subscription-lifetimes.md) | Listener identity, registration, unsubscription, and delayed callbacks |
| [Network packet budgets](references/network-packet-budgets.md) | Packet size, serialization limits, fragmentation, and bandwidth checks |
| [Replication priority scheduling](references/replication-priority-scheduling.md) | Update urgency, fairness, bandwidth allocation, and stale state |
| [Runtime sparse containers](references/runtime-sparse-containers.md) | Sparse indexing, dense iteration, mutation, and identity consistency |
| [Game object generation counters](references/game-object-generation-counters.md) | Recycled slots, stale handles, callback validation, and counter rollover |
| [Shader parameter binding](references/shader-parameter-binding.md) | Binding layouts, buffer alignment, update frequency, and validation |
| [Runtime render instance management](references/runtime-render-instance-management.md) | Instance identity, update buffers, culling bounds, and retirement |
| [Simulation checkpoint testing](references/simulation-checkpoint-testing.md) | Restore fixtures, checkpoint coverage, state comparisons, and repeatability |
| [Runtime debug command systems](references/runtime-debug-command-systems.md) | Command registration, argument validation, execution scope, and tracing |
| [Contact modification programming](references/contact-modification-programming.md) | Solver contact policy, callback restrictions, and stable contact behavior |
| [Physics world partitioning](references/physics-world-partitioning.md) | Simulation regions, boundary transfer, shared contacts, and activation |
| [Runtime object serialization](references/runtime-object-serialization.md) | Object graphs, identity, reference fixups, and partial load recovery |
| [Runtime configuration validation](references/game-runtime-configuration-validation.md) | Configuration schemas, invariants, publication, and fallback behavior |
| [Runtime prefab instantiation](references/runtime-prefab-instantiation.md) | Template expansion, staged initialization, reference fixups, and publication |
| [Asset load priority scheduling](references/asset-load-priority-scheduling.md) | Competing requests, deadlines, dependency priorities, and starvation |
| [Audio occlusion query programming](references/audio-occlusion-query-programming.md) | Query budgets, smoothing, stale results, and listener changes |
| [Audio event lifetime management](references/audio-event-lifetime-management.md) | Start and stop ownership, callbacks, looping sounds, and emitter reuse |
| [Navigation local avoidance](references/navigation-local-avoidance.md) | Steering constraints, neighboring agents, deadlocks, and path coordination |
| [AI blackboard programming](references/ai-blackboard-programming.md) | Knowledge ownership, revisions, expiry, and behavior dependencies |
| [UI list virtualization](references/ui-list-virtualization.md) | Recycled rows, stable identity, selection, and asynchronous content |
| [Runtime UI layout invalidation](references/runtime-ui-layout-invalidation.md) | Dirty propagation, measure and arrange passes, batching, and layout loops |
| [Animation event routing](references/animation-event-routing.md) | Event ownership, dispatch, filtering, cancellation, and pooled actors |
| [Runtime animation retargeting](references/runtime-animation-retargeting.md) | Skeleton mapping, scale differences, runtime constraints, and fallback |
| [Runtime audio parameter control](references/runtime-audio-parameter-control.md) | Parameter ownership, smoothing, mixing, and emitter resets |
| [Streaming audio programming](references/streaming-audio-programming.md) | Decode buffers, prefetch, seeking, underruns, and resource release |
| [Damage resolution pipelines](references/damage-resolution-pipelines.md) | Hit identity, modifiers, mitigation, health changes, and event order |
| [Target selection programming](references/target-selection-programming.md) | Candidate queries, scoring, selection stability, and invalidation |
| [Camera update pipelines](references/camera-update-pipelines.md) | Target sampling, simulation order, collision, and final pose ownership |
| [Runtime camera blending](references/runtime-camera-blending.md) | Blend state, interrupted transitions, camera spaces, and continuity |
| [Behavior tree programming](references/behavior-tree-programming.md) | Tick semantics, interruption, task state, and debugging |
| [AI perception programming](references/ai-perception-programming.md) | Sensor queries, memory, event timing, and stale observations |
| [Runtime trigger volumes](references/runtime-trigger-volumes.md) | Overlap state, enter and exit events, filtering, and lifecycle changes |
| [Gameplay interaction reservations](references/gameplay-interaction-reservations.md) | Claiming shared interaction points, contention, cancellation, and recovery |
| [Input action contexts](references/input-action-contexts.md) | Context priority, ownership, consumption, and transition behavior |
| [Gesture recognition programming](references/gesture-recognition-programming.md) | Pointer lifetimes, recognition thresholds, cancellation, and competing gestures |
| [Physics sleep and activation](references/physics-sleep-and-activation.md) | Sleep thresholds, wake propagation, activation ownership, and stability |
| [Continuous collision detection](references/continuous-collision-detection.md) | Swept motion, time of impact, fast bodies, and collision budgets |
| [Runtime texture generation](references/runtime-texture-generation.md) | Dirty regions, texture uploads, revisions, and GPU ownership |
| [GPU readback pipelines](references/gpu-readback-pipelines.md) | Asynchronous results, staging buffers, latency, and stale data |
| [Terrain editing programming](references/terrain-editing-programming.md) | Edit commands, chunk dependencies, revisions, and persistence |
| [Runtime collision cooking](references/runtime-collision-cooking.md) | Collision data generation, publication, ownership, and version consistency |
| [Asynchronous path requests](references/asynchronous-path-requests.md) | Request ownership, stale results, priorities, and cancellation |
| [Runtime navigation links](references/runtime-navigation-links.md) | Traversal eligibility, reservations, execution, and invalidation |
| [Runtime text layout and font caches](references/runtime-text-layout-and-font-caches.md) | Shaping, glyph residency, layout invalidation, and fallback fonts |
| [UI focus navigation programming](references/ui-focus-navigation-programming.md) | Focus ownership, navigation graphs, modal restoration, and dynamic lists |
| [Moving platform programming](references/moving-platform-programming.md) | Relative movement, carrying, rotations, detachment, and collision |
| [Character grounding and slopes](references/character-grounding-and-slopes.md) | Ground contact, slope classification, snapping, and movement transitions |
| [Runtime mesh generation](references/runtime-mesh-generation.md) | Geometry buffers, topology, collision updates, and resource ownership |
| [Runtime material parameter management](references/runtime-material-parameter-management.md) | Parameter ownership, per-instance values, batching, and cleanup |
| [Projectile simulation](references/projectile-simulation.md) | Fictional projectile movement, sweeps, lifetimes, and authority |
| [Melee hit detection](references/melee-hit-detection.md) | Attack windows, swept shapes, contact identity, and timing |
| [Audio voice allocation](references/audio-voice-allocation.md) | Voice limits, priorities, virtualization, and emitter lifetimes |
| [Runtime spline following](references/runtime-spline-following.md) | Distance parameterization, curve traversal, orientation, and live edits |
| [Save commit protocols](references/game-save-commit-protocols.md) | Durable staging, publication, recovery, and interrupted writes |
| [Gameplay timer schedulers](references/gameplay-timer-schedulers.md) | Timer ownership, cancellation, ordering, and simulation clocks |
| [Runtime type information](references/game-runtime-type-information.md) | Reflection metadata, type registration, serialization, and tooling |
| [Network replication layouts](references/network-replication-layouts.md) | State layouts, delta serialization, change tracking, and validation |
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
| [Frame graph profiling](references/game-frame-graph-profiling.md) | Dependency-aware CPU and GPU traces, critical paths, and pipeline stalls |
| [Background simulation tiers](references/background-simulation-tiers.md) | Update rates, state fidelity, promotion, and background progress |
| [Gameplay data binding](references/gameplay-data-binding.md) | Observing gameplay state, UI updates, subscriptions, and stale views |
| [Game asset identity and renames](references/game-asset-identity-and-renames.md) | Stable asset IDs, redirects, refactors, and reference migration |
| [Animation job pipelines](references/game-animation-job-pipelines.md) | Parallel pose evaluation, dependency barriers, and presentation handoff |
| [Gameplay invariants and validation](references/gameplay-invariants-and-validation.md) | Rules that must hold across transitions, authoring checks, and runtime diagnostics |
| [Gameplay tags and capabilities](references/gameplay-tags-and-capabilities.md) | Shared vocabulary, eligibility queries, grants, and state cleanup |
| [Gameplay transaction boundaries](references/gameplay-transaction-boundaries.md) | Atomic state changes, commit points, retries, and recovery |
| [Game content validation pipelines](references/game-content-validation-pipelines.md) | Asset rules, authoring feedback, and build validation |
| [Game thread ownership](references/game-thread-ownership.md) | Single-writer state, handoff, synchronization, and teardown |
| [Game thread backpressure](references/game-thread-backpressure.md) | Bounded work queues, overload policies, prioritization, and recovery |
| [Game resource residency](references/game-resource-residency.md) | Allocating residency budgets, evicting assets, and tracking in-flight use |
| [Game command undo and redo](references/game-command-undo-and-redo.md) | Reversible authoring actions, grouped edits, and history validation |
| [Gameplay state reconciliation](references/gameplay-state-reconciliation.md) | Server corrections, replaying pending inputs, and smoothing presentation |
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
| [Game input command history](references/game-input-command-history.md) | Tick-indexed input, command retention, acknowledgment, and replay |
| [Gameplay event ordering](references/gameplay-event-ordering.md) | Same-tick precedence, nested events, and deterministic delivery |
| [Game state diffing](references/game-state-diffing.md) | Comparing simulation snapshots and diagnosing divergent state |
| [Game resource handles and registries](references/game-resource-handles-and-registries.md) | Typed resource identity, handle resolution, reloads, and lifetime validation |
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

1. Identify the player-visible behavior and the target conditions. Read the project's instructions and identify its engine and package versions. Trace the normal entry flow to the suspected code through callers, serialized scenes, prefabs, and runtime construction where relevant. Report unresolved reachability for legacy or unreferenced code. For a regression, capture a baseline using a reproducible scene, input sequence, or playtest scenario.
2. Start with **Problem and design reasoning** in the relevant guide. Match the observed symptom and project constraints to the problem the technique addresses. Treat documented developer rationale as evidence about that game and labeled inferences as hypotheses to test. Map the chosen technique to the project's update loop, asset pipeline, or rules, and tune it using measurements from the target game.
3. Test at the normal entry point with a repeatable comparison. Use frame-time traces for performance, input and state traces for simulation and control timing, a representative playtest for experiential changes, and clean and incremental builds for pipeline changes. Record the conditions and the observed result.

For code investigations, name the mutation boundary and source location. Report source evidence, an executed isolated test, an Editor reproduction, and a device/build reproduction as separate evidence levels. Mark each as passed, failed, proposed, or unavailable with the observed result. A proposed test requiring a new helper, package, or fixture remains proposed until it runs. Prefer a fixture exercising the existing command and verify that rejected input leaves relevant state unchanged. Use disposable saves or restored test keys for persistence tests.

If symptoms overlap, start with the closest measurable cause. For a late jump, trace input arrival, eligibility, movement, and feedback timestamps before choosing between the physics and game-feel references.

## Unity workflows

| Open | When the task concerns |
|---|---|
| [Unity architecture](references/unity-architecture.md) | ScriptableObjects, event channels, object pooling, and prefab variants |
| [Unity rendering](references/unity-rendering.md) | URP pass ordering, shader variants, VFX Graph, and overdraw |
| [Unity performance](references/unity-performance.md) | Target-device profiling, import settings, quality tiers, and memory |
| [Unity UI and math](references/unity-ui-and-math.md) | UI Toolkit or uGUI, responsive layouts, debug vectors, and rotation spaces |

The [Unity book index](references/unity-books.md) identifies the supplied PDFs behind these workflows and records the relevant pages.
