# Shader compilation and warmup

## Problem and design reasoning

A frame can hitch the first time an effect appears even when the effect is inexpensive afterward. A graphics pipeline can depend on shader code, vertex layout, render-target formats, and fixed-function state. Preparing a shader alone may therefore leave work for the driver at first draw. Epic describes limitations of a bundled list learned from playthroughs for a title such as Fortnite, where content and pipeline combinations are difficult to cover exhaustively. The practical objective is to finish likely first-use work before the player needs it. [Epic's engineering breakdown](https://www.unrealengine.com/tech-blog/game-engines-and-shader-stuttering-unreal-engines-solution-to-the-problem)

## Shipped examples

- **Fortnite.** Epic uses Fortnite to explain why captured pipeline lists can have incomplete coverage. Fortnite's release notes report changes to precaching for lower-end PCs in Performance mode that reduce hitches and replace placeholders sooner. The release note ties the general precaching approach to a specific shipped path. [Epic, Unreal Fest 2025](https://dev.epicgames.com/community/learning/talks-and-demos/VEkM/unreal-engine-unreal-fest-2025-game-engines-and-shader-stutters-ue-s-solution-to-the-problem) · [Fortnite release notes](https://dev.epicgames.com/documentation/en-us/fortnite/42-00-fortnite-ecosystem-updates-and-release-notes)
- **The Last of Us Part I on PC (2023).** Naughty Dog's patch notes record reduced shader build times, a first-use crash fix, and a full shader rebuild on a subsequent patch. This shows why both fresh-cache and post-update runs are relevant even after the launch version has been tested. [Naughty Dog v1.0.5.0](https://feedback.naughtydog.com/hc/en-us/articles/15404598599444-The-Last-of-Us-Part-I-v1-0-5-0-Patch-Notes-for-PC) · [Naughty Dog v1.1.0](https://feedback.naughtydog.com/hc/en-us/articles/16476165410836-The-Last-of-Us-Part-I-v1-1-0-Patch-Notes-for-PC)

## Techniques and practical checks

### Trace compilation independently of rendering cost

Capture pipeline requests, compile start and completion, first draws, and frame times. Track cache misses separately from requests that started early but finished too late. A warm run can help distinguish first-use work from a pass that costs too much every frame.

**Check** the same route twice with cold and warm caches. Correlate first-use spikes with pipeline events and verify that repeat-use cost stays within the frame budget.

### Prepare pipelines using actual combinations

Generate likely pipeline keys from assets and render passes where practical. Collect misses from representative packaged playthroughs. Include combinations involving quality settings, decals, particles, alternate cameras, and render-target formats.

**Check** on a fresh cache with first-use effects, unusual combinations, and preset changes. Report missed keys and identify which asset, pass, or setting generated them.

### Prioritize jobs by time to first use

Prepare startup-critical pipelines first and submit later work as content becomes predictable. Limit compilation workers while gameplay is active. Choose a fallback for late work according to the asset's importance, such as a safe placeholder for decoration or delaying an encounter start for essential visual cues.

**Check** slow CPU and storage conditions and immediate entry into the first playable area. Measure startup time, compile spikes, visible placeholders, and whether essential cues are ready before play begins.

### Invalidate and recover safely

Record a cache format and build identity. Respect driver and device compatibility rules and regenerate incompatible entries. Make progress visible during longer rebuilds and leave the game able to recover from a partial cache.

**Check** a new install, an update, a settings change, and interruption during warmup. Verify resume or rebuild behavior and that subsequent launches reuse valid results.
