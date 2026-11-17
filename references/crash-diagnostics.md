# Crash diagnostics

## Problem and design reasoning

**Problem.** A crash arrives without the build or context needed to reproduce it, while raw report counts obscure which failure is spreading.

**Design reasoning.** Wube describes using automatic report frequency to prioritize player reports containing actionable detail. Bungie lists crash rates among rollout signals. Use counts to decide what to investigate and exact symbols and reproduction context to decide how to fix it.

## Shipped examples

**Factorio.** During the public 0.17 experimental release in 2019, Wube compared crash reports posted in its forum with automatically collected reports. Even when an automatic report lacked details needed for a fix, the frequency helped the team prioritize which player reports to investigate. [Wube, Friday Facts #284](https://www.factorio.com/blog/post/fff-284)

**Destiny 2.** Bungie's live release presentation lists crash rates among the signals watched after deployment, alongside player experience reports. The two sources answer different questions: the rate shows where failures are concentrated, while player accounts can reveal the action or transition immediately before a failure. [Shawna Williams, Bungie, GDC 2024](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/Williams_Shawna_Live%2BService%2BRelease.pdf)

**Ubisoft production crash data.** Ubisoft collaborators and researchers presented an empirical analysis of game crash reports at ICSE 2025. They describe crash types whose impact grows after release, including failures that remain uncommon before spreading. This motivates tracking groups over time rather than relying on a launch-day count. [ICSE 2025 Software Engineering in Practice paper](https://rebels.cs.uwaterloo.ca/papers/icse2025seip_weeraddana.pdf)

## Techniques and practical checks

### Capture enough context to explain the failure

Record a build identifier, module identifiers, failure category, platform, and recent coarse gameplay or lifecycle transitions. Use a bounded buffer written during normal execution. Keep crash-time capture minimal and use out-of-process or next-launch reporting where available. Limit uploads to approved fields and scrub sensitive data from attachments.

**Check** using a deliberate failure in a packaged test build after a known scene transition. Verify that the report survives offline restart and identifies the build and last transition. Inspect the actual payload for player text, secrets, and unnecessary identifiers.

### Match traces to the exact release

Archive the shipped binaries and symbol identifiers with the release manifest. Resolve stack addresses with matching symbols, then retain enough module and failure information for reports that cannot be fully symbolicated. Keep a separate classification for startup failures, hangs, and memory termination if the platform can distinguish them.

**Check** two packaged builds that crash in different functions. Symbolicate each using its own artifact; then supply mismatched symbols and verify the report is flagged rather than assigned a plausible but incorrect stack.

### Group and prioritize by player impact

Build fingerprints from a stable failure category and relevant stack frames. Review suspected duplicates when engine or worker entry frames hide the root cause. Segment rates by build and platform, using a denominator such as sessions or launches. Track changes in frequency and the number of affected players.

**Check** a fixture with repeated reports for one failure, two unrelated failures sharing an entry frame, and a low-volume platform with a high crash rate. Verify grouping and triage order reflect the underlying failures and exposure.

### Verify the repair through the shipping path

Create the smallest reproducible gameplay, asset, or lifecycle scenario. Run it against the affected build and the candidate on the platform that reported it. Check related transitions and watch for the original crash signature during staged rollout.

**Check** that the fixture fails on the affected build and passes on the packaged candidate. Confirm that report ingestion still receives a known test crash, so a falling production rate can be distinguished from a broken reporting pipeline.
