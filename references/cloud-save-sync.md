# Cloud save synchronization

## Problem and design reasoning

**Problem.** Two devices progress independently, or a suspended process resumes with a world in memory older than the incoming save.

**Design reasoning.** Crashlands and Hades document progress portability as a player-facing feature. A design inference is to specify the supported transfer paths and preserve both candidates until a conflict decision is durable. Device or store identity alone cannot establish which progress the player intended to keep.

## Shipped examples

**Crashlands (2016).** Butterscotch Shenanigans used a save bridge to let players move saves between the PC and mobile versions. The team's launch retrospective presents save portability as a product feature across independently purchased builds. [Samuel Coster, GDC 2017](https://media.gdcvault.com/gdc2017/Presentations/Coster_Samuel_ButterscotchShenanigans.pdf)

**Hades (2020).** Supergiant documents syncing between the Nintendo Switch and supported PC versions through cross-saves and publishes the boundaries of supported transfers. A game's supported sync paths can differ by store and platform generation. [Supergiant's Hades FAQ](https://www.supergiantgames.com/faqs/hades/)

Valve also documents dynamic sync when a handheld suspends and later resumes after progress on another device. That lifecycle means a running process may receive a new save while it still holds older state in memory. [Steamworks Dynamic Cloud Sync](https://partner.steamgames.com/doc/features/cloud#dynamic_cloud_sync)

## Techniques and practical checks

### Treat device and cloud state as separate revisions

Record a stable account scope, slot ID, schema version, and the cloud revision from which local edits started. Keep the local save usable while offline. When uploading, condition the replacement on the expected cloud revision where supported. Persist the pending edit and its operation ID so retries survive process restarts.

**Check** progress offline, quit, and reconnect. Interrupt an upload after the service accepts it but before the client receives confirmation. Confirm that retry acknowledges the existing revision and that only the intended account and slot changed.

### Preserve divergent saves until they can be resolved

Detect when two devices edited from the same base and neither copy includes the other's progress. Use a conflict screen with recognizable progress indicators or a merge only for fields with defined merge semantics. Treat the device clock as display information and preserve the unchosen copy according to a documented recovery policy.

**Check** with two offline clients and deliberately skewed clocks. Equalize playtime or timestamps while earning different rewards on each. Verify that the client identifies the conflict and that the player's chosen save contains the expected state.

### Reconcile at safe lifecycle points

Synchronize before opening a slot when possible, and publish complete snapshots at supported checkpoints, exit, or suspend. When an incoming revision arrives during play, defer application to a safe boundary or ask the player which session to continue. Group related save data into one versioned snapshot or publish a manifest only after every required file is available.

**Check** suspend on one device, progress on another, and resume the first. Interrupt a multi-file transfer and test shutdown while saving. Verify that the older in-memory state cannot overwrite the incoming revision silently and that partial snapshots never open.

### Handle compatibility and account changes

Publish a supported transfer matrix and define behavior when the receiving build lacks the save's schema, relevant content, or entitlement. Validate and migrate a copy before committing it locally. Clear account-specific caches on sign-out while preserving pending edits within the appropriate account scope.

**Check** sync from a newer build to an older build, a platform with optional content to one without it, and account switching with a pending upload. Confirm that the original progress remains recoverable and that the interface explains any blocked transfer.
