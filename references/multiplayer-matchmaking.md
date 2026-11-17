# Multiplayer matchmaking

## Problem and design reasoning

**Problem.** A queue is quick but produces uneven teams or poor connections, or tightening every constraint leaves smaller populations waiting indefinitely.

**Design reasoning.** Halo 3's TrueSkill tracking of both estimate and uncertainty and Gears 5's use of a distinct matching service suggest letting measurement and assembly change independently. Measure queue tails and match quality together before relaxing constraints.

Matchmaking assembles compatible participants and starts a session. Define which candidate properties are mandatory and which can relax as the queue ages.

## Shipped examples

**Halo 3 (2007).** Microsoft Research identifies Halo 3 as a title that used TrueSkill to rank and match players. The model represents a player’s estimated skill alongside uncertainty, which lets new and well-observed players carry different confidence even at the same estimated skill. Microsoft researchers presented large-scale analysis of Halo 3 matchmaking data at KDD 2008. [Microsoft Research TrueSkill](https://www.microsoft.com/en-us/research/project/trueskill-ranking-system/) · [KDD 2008 paper](https://herbrich.me/papers/kdd2008.pdf)

**Gears 5 (2019).** Microsoft Research says TrueMatch launched in Gears 5 and has been used with TrueSkill 2. This is an example of choosing groups through a service that consumes a skill estimate. The estimate and the choice of which group to assemble can improve independently. [Microsoft Research TrueMatch](https://www.microsoft.com/en-us/research/project/truematch/)

**VALORANT Premier (2023 onward).** Riot documents division-based matching and weekly rematch protection. The latter can relax after a team has waited in queue for several minutes. Riot’s engineering account also describes game-specific load scenarios spanning queue entry, selection, and joining a provisioned server. [Riot Premier matchmaking](https://support.riotgames.com/en-us/valorant/gameplay/premier-queues-matchmaking/) · [Riot Games engineering](https://www.riotgames.com/en/news/scalability-and-load-testing-valorant)

## Techniques and practical checks

### Make eligibility and ranking independently testable

Create tickets with compatible modes, required build or protocol, party membership, measured region latency, and eligible roles. Reject candidates that violate a hard requirement, then score acceptable candidates on skill balance, latency, queue age, or other playlist-specific preferences. Define whether mixed-skill parties are evaluated using an aggregate rating, a range, or a separate policy.

**Check** with solo and premade groups, incompatible builds, sparse role combinations, and players with conflicting region preferences. Record the reason for each rejection and confirm that search expansion never relaxes hard limits.

### Calibrate ratings and waits against observed outcomes

Keep rating uncertainty and match history available to an offline evaluator. Simulate queue arrivals across regions and population levels. Test a bounded expansion schedule that changes one preference at a time, then compare wait and quality distributions jointly. Use probability calibration to check whether predicted outcomes match observed outcomes by cohort.

**Check** peak and overnight traffic, placement matches, and high-skill tails. Compare median and high-percentile wait, cancellation, repeat pairing, latency and blowout rates. Review low-population cohorts independently so aggregate metrics cannot hide a degraded queue.

### Model handoff and recovery as state transitions

Give each ticket a clear path through waiting, reservation, acceptance, server assignment, and connection. Issue a unique match identifier and make retries idempotent. Define expiration, cancellation, and requeue behavior for players whose reservation fails or whose server cannot start. Measure wait through the first usable game state.

**Check** simultaneous cancellations, duplicate acceptance, connection failure, and server provisioning timeout. Verify that each player ends up in one match or returns to a usable queue. Check whether eligible requeued players retain the intended priority.

### Load-test the player journey

Drive test clients through realistic queue entry, parties, acceptance, game connection, and cleanup. Include burst arrivals and retry behavior. Collect stage durations and error rates so a delay can be attributed to ticketing, matching, allocation, or connection.

**Check** a launch surge, a region outage in staging, and a stalled dependency. Compare allocations with actual successful joins, confirm capacity recovers after the burst, and check that retried clients do not multiply backend requests.
