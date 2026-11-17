# Mobile device storage

## Problem and design reasoning

A game may fit when installed and still run out of space during an update because old files, new files, and temporary unpacking overlap. CD Projekt Red reported that storage and update size affected *GWENT* players and used Play Asset Delivery to reduce update downloads. Apple's WWDC session discusses background asset delivery. A practical inference is to budget peak disk use and assign separate lifetimes to saves and downloadable assets. [CD Projekt Red developer case study](https://developer.android.com/stories/games/cdpr) · [Apple WWDC25, Discover Apple-Hosted Background Assets](https://developer.apple.com/videos/play/wwdc2025/325/)

## Techniques and practical checks

### Budget peak storage

Estimate the current install, incoming assets, staging, and unpacking before importing a pack.

**Check** Test space just above and below the peak estimate. Verify the game predicts failure before staging and suggests a recovery.

### Classify file lifetimes

Keep saves and required state in durable storage and place re-downloadable packs in a separately managed cache where platform rules permit.

**Check** Clear reclaimable assets and relaunch offline. Confirm saves remain valid and unavailable content is clearly identified.

### Commit installs safely

Verify staged content and publish a new manifest only when the pack is ready. Clean up abandoned staging without removing the last playable version.

**Check** Force termination during download, extraction, and publication. Verify recovery restores a complete version.

### Expose safe reclamation

Show optional pack sizes and remove content the player selects, subject to in-use dependencies.

**Check** Uninstall a pack during and after a session. Verify state remains valid and reinstall restores access without duplicating the pack.
