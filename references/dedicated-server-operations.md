# Dedicated server operations

## Problem and design reasoning

A match can form successfully and still fail to start if no compatible server is ready. Longer placement waits, regional imbalances, and match terminations during updates indicate problems in capacity, placement, or draining. Ubisoft documented that moving *For Honor* to dedicated servers removed host migration and the resynchronizations associated with departing players. Google's game team documented a different bottleneck for its short sessions. Allocation churn taxed the orchestration control plane, so it reused server processes and distributed fleets across clusters. A transferable inference is to budget for both the number of live matches and the rate at which new matches are created.

## Shipped examples

- **For Honor (2017).** Ubisoft rolled dedicated servers to PC and then consoles in 2018. The launch notes describe eliminating host migration and related resynchronizations. At GDC 2021 the team discussed the large-scale technical test it needed to validate the migration and what it learned from running public tests. [Ubisoft release notes](https://www.ubisoft.com/en-us/game/for-honor/news-updates/2HayRoZjbJzSEJAhJMpeF7/for-honor-now-on-dedicated-servers-on-all-platforms) · [Ubisoft's GDC 2021 developer account](https://news.ubisoft.com/en-us/article/219qSeV0Zh33JOI9cKY4ly/how-for-honor-made-testing-a-celebrated-player-experience)
- **The Great Ghoul Duel 2 (2022).** The Google Doodle engineers describe pre-scaling for a short launch window, allocating from multiple server clusters behind each regional matchmaker, and returning reset game servers to the ready pool. The team tracked available and allocated servers as separate capacity measures. [Google Doodle engineering interview](https://cloud.google.com/blog/products/containers-kubernetes/making-online-containerized-games-with-managed-services)

## Techniques

### Separate readiness from process startup

Register a process as ready after it loads required data and validates the build and configuration. Allocate using a unique match request ID and a time-limited reservation. Make readiness, reservation, live match, drain, and retirement visible as distinct states.

**Check** an allocation retry after a dropped response and a process exit between reservation and join. Verify that the logical match receives one reservation at a time and an expired reservation returns capacity.

### Budget headroom by region and build

Measure time from starting a process to being ready, and safe concurrent sessions per host. Set regional headroom based on demand that can arrive during startup, with additional capacity for forecast events. Track process startup, ready count, placement delay, and unsuccessful joins by region and build.

**Check** a synthetic ramp and a sudden traffic burst in staging. Verify that placement delay and server tick time stay inside their budgets. Repeat with a cold build cache and one region unavailable.

### Drain server builds gradually

Route a small share of new matches to a candidate build and monitor its placement, connection, completion, and tick-time metrics. Keep existing sessions on compatible servers until they finish. Withdraw the candidate from new placements if it violates the thresholds and retain enough previous-build capacity to recover.

**Check** a rollout followed by rollback during both short and long matches. Verify that new matches return to the previous build, and that existing matches finish or follow the explicit maintenance policy.

### Validate isolation between reused sessions

Reset rosters, timers, transient gameplay state, and process-level caches that belong to the last match. Return a process to the ready pool after confirming the reset. Retire processes that cannot recover cleanly or whose memory grows steadily.

**Check** back-to-back sessions with different rosters and maps, followed by a long soak. Confirm the next session sees only its own state, results are submitted once, and memory remains within budget.
