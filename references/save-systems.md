# Saves and persistent world state

## Problem and design reasoning

**Problem.** Progress reappears after being consumed, world references break across load boundaries, or a crash leaves an unreadable save. Compare what the player expects to persist with what the game actually commits.

**Design reasoning.** Housemarque documents Returnal's single-use suspend behavior. An inferred general lesson is to decide what a resume promises first, then choose snapshot, migration, and write recovery behavior to uphold it. See the shipped examples below for sources.

## Shipped examples

**Doom (1993).** The released Linux port archives players, mutable map data, active thinkers, and special effects in separate passes. It restores those categories onto a loaded map. Static content can come from the original level while the save records state that changes during play. [id Software save code](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/p_saveg.c)

**Returnal (2021).** Housemarque added a single-use suspend point that lets a player turn off the console and resume a cycle. Some sequences disallow suspension to preserve the intended flow. Choose save semantics around what the player expects to retain and when they are allowed to resume. [Housemarque update announcement](https://blog.playstation.com/2021/10/26/returnal-2-0-update-brings-suspend-cycle-and-photo-mode/)

**Minecraft Java Edition.** Mojang documents that its DataFixerUpper library supports incremental transformations between game-data versions. This allows format changes to be expressed as migration steps. [Mojang DataFixerUpper](https://github.com/Mojang/DataFixerUpper)

## Techniques and practical checks

### Give persistent objects stable identities

Use authored IDs for placed objects and persisted IDs for dynamically created objects. Save changes by ID and represent deletion explicitly when loading the base cell would recreate a destroyed object. Keep deferred references as IDs until their target cell loads. Reconcile cooked content and save deltas before exposing the cell to gameplay.

**Check** by moving or deleting a prop, unloading its cell, saving, and resuming from a distant region. Return by multiple routes and verify that the prop appears exactly once in its intended state. Repeat with a quest reference to the unloaded cell.

### Snapshot at an explicit save boundary

Choose which values persist across levels, checkpoints, or interrupted sessions. Build a coherent snapshot at a simulation boundary. Serialize that snapshot asynchronously if necessary. Publish it through a platform-supported transactional write or a temporary-file commit, and retain a prior valid generation where recovery semantics permit it.

**Check** by interrupting writes at multiple stages and restarting. Confirm that the game selects a complete generation and reports recovery when appropriate. Include full storage and two overlapping autosave requests.

### Version data and test upgrades

Store a schema version with the save. Translate older structured data before instantiating gameplay objects. Define how renamed or deleted content IDs map to current content. Preserve the original save until the migrated replacement commits.

**Check** with fixtures from every supported format, including a missing optional field and removed content. Reload a migrated save and compare quest variables, inventory, and modified world entities with expected values.

### Verify the semantics of resuming

Define whether a save can be loaded repeatedly and what happens when a resume fails. Specify which pending actions, timers, or spawned events should replay and which are already committed. Establish a recovery position when a level change invalidates the recorded location.

**Check** repeated save and reload around a reward, checkpoint, and region transition. Confirm that rewards apply once, required events remain reachable, and single-use records follow the intended consumption policy.
