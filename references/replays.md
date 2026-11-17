# Replays, spectators, and highlights

## Problem and design reasoning

**Problem.** Players need to inspect an event after it happened, while support teams need recordings that survive reproduction and seeking. Decide what a viewer must be able to reconstruct beyond a fixed camera feed.

**Design reasoning.** Blizzard describes supporting several replay use cases on shared technology. Doom's compact input stream illustrates the complementary trade-off of relying on compatible simulation and content to keep recordings small. See the shipped examples below for sources.

## Shipped examples

**Doom (1993).** The released Linux port records per-tick commands and playback reads those commands into the simulation. Its demo header also carries setup data. This is compact when the same starting state and command stream produce the same outcome. [id Software, game and demo code](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/g_game.c)

**Overwatch (2016).** Blizzard's replay talk describes a shared technology supporting death recaps, highlights, playback, and bug reproduction. These consumers differ in how much history and context they need. [Blizzard developer presentation, GDC 2017](https://www.gdcvault.com/play/1024053/Replay-Technology-in-Overwatch-Kill)

**Fortnite (2017 onward).** Epic documents replay camera controls that let viewers follow players or teams and change viewpoints in recorded Creative games. Epic also documents that previous-update replays may become unavailable, illustrating the need for an explicit compatibility policy. [Epic replay controls](https://dev.epicgames.com/documentation/en-us/fortnite/replays-feature-in-fortnite-creative) · [Epic compatibility support](https://www.epicgames.com/help/c-ategory_fortnite/c-fortnite_gameplay/a25553277)

## Techniques and practical checks

### Choose what to record

Capture tick-indexed inputs and initial state when the relevant simulation is reproducible. Capture timestamped authoritative state or state deltas when playback needs to reproduce outcomes independently of a local simulation. A hybrid can use periodic complete snapshots and deltas or inputs between them. Include game build, content identifiers, simulation rate, and recording format version.

**Check** the final state and selected transition ticks against the live run. Exercise spawning, teleports, and late joins. Compare bytes per minute and recording cost in both a quiet match and the busiest supported scenario.

### Make seeking explicit

Index checkpoints by time. To seek, restore the nearest prior checkpoint and apply intervening records. Suppress historical one-shot audio and effects during fast-forward, while reconstructing effects that should still be active at the destination. Resume normal event dispatch after landing on the requested tick.

**Check** the same destination through uninterrupted playback, a forward seek, and a backward seek. Compare actor state, camera targets, and active effects. Measure worst-case seek delay and validate the index when a recording ends unexpectedly.

### Treat camera and visibility as playback policies

Keep camera choice independent from simulation. Design follow mode, free camera, and highlight framing around the information available to the viewer. In a competitive game, apply team visibility, delay, and reveal rules before sending live spectator data. Give highlights a small window before and after the trigger so the viewer can understand cause and effect.

**Check** target death, target removal, view switches, and several highlight triggers close together. In an authorized local match, verify a team-restricted spectator receives only the state allowed by the viewing policy.

### Define upgrade behavior

Maintain a sample replay for each format or build range the game promises to support. Choose between migrating old data, keeping a compatible playback path, or offering rendered exports when long-lived sharing matters. Detect incompatible content before trying to simulate it.

**Check** a current recording, an older supported recording, a deliberately incompatible build, and a truncated recording. Confirm graceful rejection with a clear reason wherever playback is unsupported.
