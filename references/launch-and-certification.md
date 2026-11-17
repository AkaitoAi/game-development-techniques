# Launch and platform certification

## Problem and design reasoning

**Problem.** A game passes from a developer build, but a fresh store install, launch update, or cross-platform connection breaks a player's first session.

**Design reasoning.** Hello Games' launch update and Arrowhead's launch experience point to different conditions a release rehearsal should cover. Sample the exact packages, entry paths, and load conditions real players will encounter.

## Shipped examples

**No Man's Sky (2016).** Hello Games documented its first update just before launch. It changed world generation and advised starting over if an early save had already been made. The disc build, launch update, and pre-release saves therefore needed distinct test paths. Sean Murray later participated in GDC 2019's developer-journey main stage. [Hello Games, Update 1.03](https://www.nomanssky.com/2016/08/update-1-03/) · [GDC 2019, The Developer's Journey](https://gdcvault.com/play/1025975/GDC-Main-Stage-The-Developer)

**Helldivers 2 (2024).** PlayStation's launch announcement identified simultaneous PS5 and PC availability and crossplay. Arrowhead's retrospective describes the surge in attention at launch. A cross-platform launch has multiple entry points to the same player experience, so a successful launch on one storefront only covers part of the release. [PlayStation launch announcement](https://blog.playstation.com/2023/09/14/helldivers-2-launches-february-8-2024/) · [Arrowhead, one year later](https://blog.playstation.com/?p=401049)

## Techniques and practical checks

### Translate requirements into testable evidence

Use the target storefront's review and platform requirements as the source of truth. Record each applicable requirement, owner, scenario, environment, candidate identifier and evidence. Cover suspension, account changes, lost connectivity, controller handoff and storage failures where relevant. Check the current requirements for each submission. [Valve review process](https://partner.steamgames.com/doc/store/review_process)

**Check** the shipping candidate on a clean account and hardware profile. Trigger each supported lifecycle event during loading, gameplay and saving. Verify safe recovery and capture the build ID and observed result.

### Pair the store page with the candidate

Maintain a matrix of advertised features by platform, region and edition. Assign each listed feature a test in the submitted build. Track store page approval, ratings, package approval and publication as separate gates where the platform treats them that way. [Valve release process](https://partner.steamgames.com/doc/store/releasing)

**Check** every public claim against the designated playable build. Test correct editions, languages and optional content entitlement from a fresh account and check that prompts and store destinations match its platform.

### Rehearse release-day dependencies

Schedule unlock, entitlement activation and service readiness. Assign ownership for each gate and define what can proceed independently. Where a retail build or preload precedes an update, explicitly test first boot before and after the update.

**Check** a release rehearsal with one platform delayed and an online dependency degraded. Confirm that status messages fit what players can access and that the launch team can pause the affected gate.

### Establish a visible go/no-go decision

Define launch blockers, acceptable degradation, and who can publish or stop rollout. Prepare first-hour checks for purchase, download, boot and first successful gameplay. For shared services, record expected wait times or failure rates and who responds when they exceed limits.

**Check** an operator handoff with a failed login or entitlement smoke test. Verify that the next operator can identify the exact failing step, stop publication where supported and communicate player impact.
