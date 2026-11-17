# Sky and weather

## Problem and design reasoning

A sky, sun, fog, and ambient lighting can each be plausible in isolation yet disagree about the hour or weather. Sucker Punch wanted continuous time of day, dynamic weather, and the expressive atmosphere of samurai cinema. Guerrilla's later voxel cloud work also had to hold up when players could fly through clouds. The documented targets differ. One practical inference is to choose atmospheric complexity from what the player can see and traverse, then share the resulting environment state across the rendering systems that express it. [Sucker Punch, Real-Time Samurai Cinema, SIGGRAPH 2021](https://advances.realtimerendering.com/s2021/jpatry_advances2021.pdf) · [Guerrilla, Nubis³, SIGGRAPH 2023](https://advances.realtimerendering.com/s2023/index.html)

## Shipped examples

- **Ghost of Tsushima (2020).** Sucker Punch documented a continuous time of day with dynamic weather and atmosphere. Its rendering talk describes lighting haze, clouds, and particles with multiple scattering and deriving indirect lighting from sky visibility. [Jasmin Patry, Real-Time Samurai Cinema, SIGGRAPH 2021](https://advances.realtimerendering.com/s2021/jpatry_advances2021.pdf)
- **Horizon Forbidden West: Burning Shores (2023).** Guerrilla's Nubis presentation describes voxel clouds that can serve as a time-of-day sky from the ground and be traversed on flying mounts. The team accelerated cloud ray marching with compressed signed distance fields and addressed the bandwidth cost of accessing dense voxel data. [Andrew Schneider, Nubis³, SIGGRAPH 2023](https://advances.realtimerendering.com/s2023/index.html)

## Techniques and practical checks

### Share weather state across systems

Define time of day, sun direction, cloud coverage, precipitation, visibility, and wind as shared inputs. Give each downstream system its own response curve while keeping the same source state. Define what a scripted sequence can override and how it returns to the world state.

**Check** dawn, midday, dusk, and a storm transition in the same location. Inspect shadow direction, sky brightness, fog tint, and precipitation together. Activate and release a scripted override and verify that the environment converges on the intended live state.

### Pick cloud representation by travel range

Use sky textures or layered cloud fields where the camera remains far away and parallax is limited. Test a bounded volume for clouds the player enters or passes closely. Set a GPU and memory budget before increasing density resolution or march steps. Allow lower tiers to approximate inaccessible clouds.

**Check** the same formation from the ground, from above, and through its boundary when supported. Record horizon seams, density changes, and GPU cost. Verify that the intended cloud silhouette survives quality changes.

### Blend weather by region and time

Define weather transitions as authored changes in state with separate timing for fast effects and slow responses such as wetness. Sample spatial weather at the relevant listener, camera, or actor and use consistent rules at borders. Decide what data a save or replicated session needs to restore.

**Check** walk across a regional boundary, fast-travel into ongoing rain, and reload a save halfway through a transition. Verify there are no contradictory wetness, precipitation, or audio states and that weather resumes correctly.

### Validate temporal stability and gameplay visibility

Measure atmospheric rendering during camera movement and rapid weather changes, where temporally accumulated effects can lag. Define when changes such as teleportation should reset history. Tune fog and precipitation against the contrast required for routes and critical game cues.

**Check** sunset time-lapse, a fast camera pan, a teleport, and the busiest storm scene. Record GPU frame time, visible trails, exposure recovery, and whether essential landmarks can still be recognized at intended distances across presets.
