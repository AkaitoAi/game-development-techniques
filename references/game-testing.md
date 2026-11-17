# Game testing and regression automation

## Problem and design reasoning

**Problem.** A familiar route passes in a short manual test, but long traversals, multiplayer timing, and intermittent stalls keep escaping reproduction.

**Design reasoning.** Ubisoft Reflections' bots followed player input paths and reported incomplete missions. The production lesson is to automate repeatable coverage along normal player routes and keep the failing sequence available so a person can investigate the result.

## Shipped examples

**Doom (1993).** The released Linux port records and replays per-tick player commands. The timed demo path runs a recorded sequence and reports elapsed real time alongside simulated ticks. A stable input recording can drive a regression test or benchmark without asking a tester to reproduce each movement by hand. [id Software, released game code](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/g_game.c)

**Tom Clancy's The Division (2016).** Ubisoft Reflections developed client bots that mimic player input and report problems such as incomplete missions. The team used the system for mission traversal, issue reproduction, multiplayer scenarios, and gathering performance data while traversing streets. [Ubisoft Reflections, GDC 2019](https://gdcvault.com/play/1026069/Automated-Testing-Using-AI-Controlled)

**Fortnite.** Epic describes using the workflows it later packaged as Horde to ship Fortnite and Unreal Engine. Its Unreal Fest session covers build automation, automated test management, and managing development kits. This supplies an example of organizing release infrastructure around test execution and hardware access across projects. [Epic Games, Unreal Fest 2024](https://dev.epicgames.com/community/learning/talks-and-demos/9dP9/unreal-engine-horde-and-unreal-build-accelerator-operating-at-epic-scale-unreal-fest-2024)

## Techniques and practical checks

### Assert game state at the cheapest useful layer

Exercise isolated rules with a controlled state, then use packaged builds for representative sequences where input, assets, systems, and timing meet. For a quest reward, assert inventory and quest transitions separately, then interact through the shipped UI to catch integration problems.

**Check** an expected success, full inventory, repeated interaction, and interruption between grant and quest completion. Deliberately break a fixture to confirm that the failure names the rule or transition that failed.

### Preserve repeatable input and initial state

Capture the build and content versions, initial snapshot, simulation clock, inputs, and random seeds needed for a replay. Add state assertions at decisive transitions. Use tolerances where the simulation or graphics output legitimately varies across platforms.

**Check** the same recording repeatedly on a clean build and after loading the saved fixture. If checks diverge, record the first failing tick and the relevant state. For benchmarking, hold the route and settings constant and track frame-time percentiles across runs.

### Cover long gameplay routes with observable bots

Give a bot a short sequence of goals and have it act through ordinary player commands where input integration is part of the test. Use a deadline per goal and capture the last successful transition, objective state, game logs, and position when it stalls.

**Check** one known route and fixtures with a blocked doorway, delayed spawn, and missing objective. Confirm that the report distinguishes completed goals, timeouts, and setup errors. Keep human playtests for whether the route teaches the intended behavior or feels fair.

### Split quick checks from expensive scenarios

Run a small packaged smoke suite for build, boot, input, and first playable content. Run broader platform and multiplayer scenarios on representative hardware. Use per-build artifacts so a failing session can be started from the same version and inputs.

**Check** a fresh install, an upgrade with existing saves, and a deliberately unavailable dependency. Require reports with the build ID, device, scenario, and outcome. Distinguish infrastructure failures from reproducible game failures before counting regressions.
