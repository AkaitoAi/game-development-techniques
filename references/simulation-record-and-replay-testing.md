# Simulation record-and-replay testing

## Problem and design reasoning

Timing and ordering defects can disappear when a tester repeats a scenario by hand. A simulation regression trace captures the starting conditions and ordered inputs needed to drive a test runner. The runner compares outcomes at defined ticks and reports where they first differ.

**Shipped examples.** In his GDC 2017 talk, Blizzard's Philip Orwig identified bug reproduction as an internal use for Overwatch's replay technology. Wube described recording and replaying Factorio sessions to test for divergence and reported capturing paired saves and replay data for desync investigation. The design implication is to preserve enough context to regenerate or localize a failure. The appropriate trace format depends on which state and external inputs are under the test's control. [Philip Orwig, Replay Technology in Overwatch, GDC 2017](https://www.gdcvault.com/play/1024053/Replay-Technology-in-Overwatch-Kill) · [Wube, CRC fun](https://www.factorio.com/blog/post/fff-47) · [Wube, First MP Game](https://www.factorio.com/blog/post/fff-51)

## Techniques and practical checks

### Define what the trace can reproduce

Record the starting snapshot or deterministic setup, build and content identifiers, simulation settings and random seeds. Capture tick-stamped commands and external outcomes that can influence simulation, including asynchronous completions where relevant. Replay simulation inputs through the same entry points that consume them in normal play.

**Check** replay the same trace repeatedly at different render rates with external services stubbed. Verify stable state at selected ticks. Run with an incompatible content manifest and expect an explicit compatibility result.

### Compare state at meaningful boundaries

Digest only stable simulation fields in a canonical order, then capture focused field diffs when digests differ. Add domain assertions for important outcomes such as contact, ownership, sequence completion or an exactly-once reward. Compare implementation-sensitive digests within compatible builds and use semantic expectations across intentional implementation changes.

**Check** inject a one-tick ordering defect and verify that the runner identifies the earliest observable divergence and maps it to the affected entity or subsystem. Repeat with a presentation-only change and confirm the intended assertions remain stable.

### Reduce failing traces carefully

Keep the original trace and save a separate minimized fixture. Remove unrelated phases and commands only while testing that the same failure signature remains. Place a checkpoint inside a long setup only when it carries enough state for the failure to reproduce after restore.

**Check** run the minimized fixture against an affected build and the fix. Expect the same assertion or field-level divergence before the fix and a passing result afterward. Restart the process to confirm the fixture has no hidden setup dependency.

### Run isolated tests and keep useful evidence

Route file writes, telemetry and network side effects to isolated test adapters. Version the trace format and give each fixture an owner, scenario name and expected result. Keep fast traces on the normal build path and schedule longer or more varied cases as coverage demands.

**Check** fail a controlled test run and inspect the resulting artifact. Confirm it includes trace version, tick, build, subsystem and expected versus actual state, can rerun independently, and contains no real player account data.
