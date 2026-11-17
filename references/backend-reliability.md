# Game backend reliability

## Problem and design reasoning

**Problem.** Launch demand exceeds capacity, retries amplify overload, or a shared dependency takes otherwise healthy parts of the game offline.

**Design reasoning.** Google's Pokémon GO account documents retry amplification. Roblox identified dependency isolation and independent observability as recovery improvements. The operational reasoning is to bound failure propagation and measure whether the player journey completes, even when an optional dependency does not.

## Shipped examples

**Ingress (2012) and Pokémon GO (2016).** Niantic's Edward Wu described how both games maintain a shared world using scalable databases and containerized services. For Pokémon GO, Google engineers reported that launch traffic reached nearly 50 times their initial estimate. Synchronized client retries amplified load while unhealthy backends were slow to time out. Actual request demand can exceed measured throughput when clients are waiting or retrying. [Niantic, GDC 2017](https://www.gdcvault.com/play/1024364/Creation-of-Planet-Scale-Shared) · [Google SRE case study](https://sre.google/workbook/managing-load/)

**Roblox (2021 outage).** Roblox's incident review describes a shared service-discovery cluster that affected many services when it degraded. Some monitoring also relied on affected infrastructure. The team identified isolation and independently available observability as recovery improvements. [Roblox and HashiCorp incident review](https://about.roblox.com/newsroom/2022/01/roblox-return-to-service-10-28-10-31-2021)

## Techniques and practical checks

### Budget the player journey

Map dependencies for actions such as starting a match, loading a profile, and committing a reward. Assign latency and failure budgets to the full action. Identify which dependencies are essential and where a feature can degrade. For example, a game may continue a current match during loss of a cosmetic catalog and show a cached catalog with clear freshness information.

**Check** each dependency in a staging environment with latency, timeouts, and unavailability. Measure player-visible completion and tail latency. Verify that optional failures leave essential actions usable and that uncertain results are displayed accurately.

### Bound retries and absorb launch spikes

Set request deadlines and a finite retry budget. Stagger retries using backoff and jitter. Limit queued work and prefer an explicit busy result when the next request cannot finish within its budget. Use a stable operation ID for retryable state changes so a delayed response cannot duplicate rewards or inventory changes.

**Check** a launch-shaped traffic ramp and recovery in a controlled environment. Track original requests, retries, accepted work, and queue age. Simulate a timeout after a reward commits and confirm the retry returns the existing result.

### Limit the impact of shared dependencies

Identify dependencies shared by all regions or features and decide which failures need isolation. Give compartments independent capacity and bounded interaction. Make status, diagnostics, and the minimum recovery controls available during loss of the affected service.

**Check** a simulated regional outage and a shared control-plane outage. Verify that unaffected sessions continue within their budgets and operators can still see current impact, change routing, and confirm recovery.

### Recover without dropping persistent changes

Keep release health metrics visible by build and region. Document how to stop traffic to an unhealthy release and what happens to requests that were already accepted. Separate rollback of executable code from recovery of persisted state and check that older code can read any data already written by a canary.

**Check** a failed canary followed by a staged restart. Reconcile accepted requests and committed records. Confirm that the player can resume the intended action, with no missing or repeated progression, before restoring normal traffic.
