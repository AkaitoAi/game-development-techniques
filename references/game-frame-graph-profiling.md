# Game frame graph profiling

## Problem and design reasoning

A busy frame can contain overlapping simulation, animation, render preparation, and GPU execution. Adding the duration of every marker double-counts overlap. Reading only the main thread can hide a worker that gates frame submission or a queue that leaves the GPU waiting. Follow the dependencies that determine the frame's completion.

In *Destiny*, Bungie built a job graph across most of the engine to exploit available parallelism while supporting low-latency gameplay. Chris Butcher's and Barry Genova's presentations describe the engine and its job architecture. [Bungie at GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-) · [Destiny engine slides](https://media.gdcvault.com/gdc2015/presentations/Butcher_DestinyEngine_GDC2015_final.pdf)

In *Killzone: Shadow Fall*, Guerrilla's presentation on threading entity updates includes a global frame profiler timeline across the main thread and workers. A practical inference from these talks is to correlate task timing with dependency edges before changing the job that appears to cost the most in aggregate. [Jorrit Rouwe at GDC Europe 2014](https://www.guerrilla-games.com/media/News/Files/Rouwe_Jorrit_Threading_Entity_Update.pdf)

## Practical checks

- **Check** trace a stable scene and a repeatable hitch with frame IDs on CPU tasks, GPU submissions, and presentation events. Account for buffered frames before associating a CPU task with a GPU spike.
- **Check** add a controlled delay to a job gating submission in a development build and inspect the dependent waits. Compare with the same delay in a job that finishes within its existing slack.
- **Check** instrument when tasks become eligible, when they start, and when they finish. During the hitch, distinguish a job waiting on input data from a ready job waiting for an available worker or an explicitly serialized resource.
- **Check** compare the same capture before and after optimization. Use high-percentile frame latency and missed deadlines alongside average time. Confirm any improvement survives with detailed capture disabled.
