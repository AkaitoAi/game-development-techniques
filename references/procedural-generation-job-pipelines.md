# Procedural generation job pipelines

## Problem and design reasoning

A procedural chunk can spend little time generating its source data yet still hitch when geometry, collision, assets, and entities become ready together. In *No Man's Sky*, Innes McKendrick describes a continuous pipeline from voxel generation through polygonization, texturing, population, and simulation so a small team could support seamless planetary exploration. Bungie's *Destiny* work describes a job graph with runtime validation of safe data creation and destruction. The portable recommendation is to represent generation as staged work with explicit output revisions and an integration budget. These contracts are inferred engineering guidance, rather than an account of Hello Games' exact job API. [McKendrick, Continuous World Generation in No Man's Sky, GDC 2017](https://www.gdcvault.com/play/1024265/Continuous-World-Generation-in-No) · [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

## Techniques and practical checks

### Separate generation stages from world publication

Give each stage immutable inputs and owned outputs, such as a density field, mesh, collision description, or placement list. Express dependencies before running jobs. Let workers produce staged data, then publish through the frame phase that owns each destination system. Define which outputs must appear together for a chunk to be playable.

**Check** delay geometry, collision, and placement independently. Verify gameplay cannot enter a chunk whose required representation is incomplete and that publishing ready chunks stays within the integration budget.

### Derive work identity from generation inputs

Associate the world seed, chunk coordinate, generator version, relevant content revision, and edit revision with each request. Give random substreams stable stage identifiers so scheduling order does not change generated choices. Validate the request revision when a worker completes.

**Check** generate the same corpus using different worker counts and shuffled stage completion. Compare canonical outputs, then edit one chunk while its old work runs and verify the stale result is discarded.

### Bound the entire pipeline

Limit queued chunks, intermediate bytes, and per-stage concurrency. Prioritize the route the player is likely to reach, with bounded service for lower-priority work. Propagate cancellation through dependent stages and reclaim their temporary buffers only after active workers have released them.

**Check** teleport repeatedly between distant regions while workers are intentionally slowed. Record peak staged memory, queue age, cancelled work, and time to playable terrain. Verify resource use returns to the expected baseline.

### Validate borders and failing seeds

Specify how adjacent chunks obtain their common boundary samples and placement ownership. Record the seed and input revisions for failures and maintain a small regression corpus containing difficult boundaries and edited regions.

**Check** generate neighbors in reverse order and at supported detail levels. Inspect seams, duplicated props, traversal continuity, and collision at the shared boundary, then reproduce a saved failing case in a clean run.

See [Procedural generation](procedural-generation.md) for design guarantees and [Runtime collision cooking](runtime-collision-cooking.md) for collider publication.
