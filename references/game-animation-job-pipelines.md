# Game animation job pipelines

## Problem and design reasoning

An animation job can finish late or read a character that changed while it ran. A barrier that waits for every character can also delay gameplay and rendering even when most poses are ready. Bungie describes organizing most of the shipped *Destiny* engine around job graphs, with low-latency action as a design goal. The pose-specific producer and consumer rules below are a proposed application of that architecture. [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-)

Naughty Dog describes increasing the number of NPC characters shipped in *Uncharted 2* while improving CPU and memory efficiency and maintaining the animation team's size. This makes character throughput and iteration useful outcome measures alongside visual quality. The talk summary does not establish that *Uncharted 2* used the particular job layout described below. [Naughty Dog, GDC 2011 animation presentations](https://www.naughtydog.com/blog/gdc_2011_day_two)

## Techniques and practical checks

- **Snapshot the inputs for a pose job.** Capture the simulation tick, rig and clip references, parameters, and transforms the job reads. Hold resources through completion and store output separately from the last published pose. **Check** switch clips, replace a rig, and despawn a character with work in flight. Verify that a late result cannot publish into a recycled character slot.
- **Assign a deadline to each consumer.** Complete the work needed for root motion and gameplay contacts before their simulation consumers. Let cosmetic-only consumers reuse the last complete pose where the result is acceptable. Publish complete poses with their source tick. **Check** delay pose work in a test scene and confirm events and collisions keep their defined tick while the renderer follows the documented fallback.
- **Choose job size with a workload capture.** Batch small evaluations when dispatch dominates and split batches that leave slow stragglers. Keep clip sampling, blend, and IK dependencies explicit. **Check** measure few-character and crowd scenes with the same camera path. Compare dispatch overhead, time waiting at barriers, and the longest animation job before and after changing batch size.
- **Make quality transitions observable.** Document which characters can update less frequently or skip cosmetic solvers and which gameplay consumers always need current results. **Check** move a character quickly from distant to close range while a climb or turn is playing. Inspect update cadence, contact positions, and visual pops across the transition.
