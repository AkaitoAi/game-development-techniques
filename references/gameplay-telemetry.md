# Gameplay telemetry and playtest analysis

## Problem and design reasoning

**Problem.** A studio can see that a build performs poorly or that deaths are common, but it cannot tell which place, build, or encounter to investigate.

**Design reasoning.** Guerrilla's combination of gameplay and game-health observations in world-map tools suggests asking what investigation the record must support. Keep location and build context close to each measurement so a team can reproduce it.

## Shipped examples

**Halo 3 (2007).** Bungie reported publishing map overlays for kill and death locations with weapon filters and personal and aggregate views. At launch of the feature it processed a subset of the data and prioritized core statistics and file sharing ahead of heatmap processing. Even a helpful analysis tool needs a defined collection scope and workload budget. [Bungie's November 2007 weekly update, archived by Halo.Bungie.Org](https://halo.bungie.org/bwu/index.html?item=153)

**Horizon Zero Dawn (2017).** In a CEDEC retrospective, Guerrilla's Michiel van der Leeuw describes world-map tools and game-health telemetry that combined observations from gameplay, developers and automated test runners. Attaching measurements to where they occur can turn a studio-wide trend into an actionable content or performance issue. [CEDEC 2019, Making Tools for Big Games, slides and notes](https://d3d3g8mu99pzk9.cloudfront.net/MichielVanDerLeeuw/CEDEC%202019%20-%20Making%20Tools%20for%20Big%20Games.pdf)

## Techniques and practical checks

### Instrument a design question

Write the decision first. For example, to decide whether a tutorial hint arrives too late, record when a player reaches its trigger, sees the hint and succeeds or leaves. Include build, content revision and an attempt or session ID. Use the smallest payload that answers the question. Exclude personal data that is unnecessary for analysis and keep development and public datasets distinct.

**Check** runs for success, failure, retry, skip and disconnect. Confirm event meaning and sequence against a developer replay or log. Identify which partial sessions contribute to the denominator and why.

### Measure exposure alongside outcomes

Count eligible attempts, players entering a region or time spent there, depending on the question. A busy room can have more deaths even when it is safer per entry. For a map overlay, save stable position coordinates or a region ID and associate them with the map revision. Report rates and sample sizes by relevant cohorts before comparing levels or builds.

**Check** with equal failure rates across two fixtures with unequal traffic. Verify that raw counts differ but normalized rates agree. Test different elevations in the same horizontal cell and a map update that changes coordinates.

### Budget and monitor delivery

Record each event's schema version, stable identity and source clock. Batch uploads away from frame-critical work and bound the queue. Decide which events to sample or drop under pressure and record collection gaps. Deduplicate retries where counts matter. Use local structured logs for an offline workbench or a small playtest.

**Check** with offline sessions, a slow receiver, reordered batches and duplicate events. Confirm that gameplay stays inside budget and that dashboards make incomplete collection visible.

### Verify an interpretation with a replay or playtest

Treat a cluster or drop-off as a clue about where to inspect. Examine selected attempts in sequence, or observe a comparable set of players, to distinguish navigation, comprehension and execution problems. Change one candidate cause and compare the target metric with a guardrail such as completion time or experienced difficulty.

**Check** a controlled change with known event counts and confirm the analysis detects it. Compare similar cohorts on the same map and report uncertainty when the sample is small. Verify that a change helping the target group preserves intended outcomes for the other groups.
