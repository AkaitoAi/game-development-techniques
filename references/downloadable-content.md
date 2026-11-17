# Downloadable content

## Problem and design reasoning

**Problem.** A base install depends on absent files, content ownership disagrees with installation state, or uninstalling an add-on strands a save.

**Design reasoning.** CD Projekt Red documents separate and bundled Witcher 3 packaging and asymmetric Phantom Liberty save compatibility. A design inference is to design package boundaries and save behavior together and make requirements visible before the player commits progress.

## Shipped examples

- **The Witcher 3: Wild Hunt.** CD Projekt Red offered a Switch Complete Edition containing the base game and expansions and later made those expansions available as separate products. A GDC 2017 talk on building Beauclair describes the production of the capital city in the *Blood and Wine* expansion. Together, the sources illustrate how a substantial new playable area can also have multiple distribution arrangements. [CD Projekt Red Switch announcement](https://www.thewitcher.com/us/en/news/32870/base-game-and-expansions-available-separately-on-nintendo-switch) · [Building Beauclair, GDC 2017](https://gdcvault.com/play/1023965/Building-Beauclair-Capital-City-in)
- **Cyberpunk 2077: Phantom Liberty (2023).** CD Projekt Red's PAX East panel describes the expansion's production and distinct spy-thriller direction. The studio's support documentation also specifies asymmetric save compatibility. A base-game save works with the expansion installed, while a save created with the expansion installed requires it even when the player has yet to enter its content. [CD Projekt Red developer panel, PAX East 2024](https://press.cdprojektred.com/en/news/1552/cyberpunk-2077-phantom-liberty-pax-east-2024-panel-now-available-online) · [CD Projekt Red save compatibility](https://support.cdprojektred.com/en/cyberpunk/pc/sp-technical/issue/2960/i-am-unable-to-load-my-save-from-cross-progression-1)

## Techniques

### Draw a package boundary

Map a stable content ID to the required bundles, supported base builds, and any other content dependencies. Shared systems that the base game needs belong in the base build. Resolve optional registries through explicit extension points. Validate the dependency graph independently for each storefront package configuration.

**Check** a clean base install alongside a bundled edition and a separately installed expansion. Scan for required references from the base build into absent content, duplicate registries when both packages are present, and incompatibilities with the oldest supported base build.

### Track ownership and readiness independently

Distinguish entitlement, install progress, integrity, compatibility, and activation. Give the UI a specific message for each blocked state. Refresh account and package state at appropriate lifecycle boundaries, and apply the platform's documented offline-entitlement behavior.

**Check** an owned but uninstalled pack, paused installation, completed download awaiting activation, and revoked entitlement. Switch accounts where supported. Verify that menus and gameplay agree about availability and that an installed payload cannot bypass ownership checks.

### Decide what happens to saves when content goes away

Write explicit content and format requirements into save metadata as needed. Choose whether a save with unavailable dependencies can load in a defined fallback mode or needs the exact content restored. Preserve the save while offering recovery, and keep the restore operation safe to retry.

**Check** by saving before entry, inside DLC, and after acquiring a DLC-dependent item. Remove and reinstall the pack. Verify that each save either recovers with its state intact or explains exactly which content is needed. Repeat after loading an older base-only save.

### Stage activation and verify mixed installations

Use atomic or platform-managed package activation and defer registry changes to a safe boundary. Publish by package and compatible base version. Document multiplayer entry rules for mixed ownership and support reconnect after an interrupted update where applicable.

**Check** an interrupted update, low disk space, stale catalog data, and mixed base/DLC matchmaking on supported platforms. Confirm that retry does not duplicate content and that incompatible sessions explain the missing requirement before entry.
