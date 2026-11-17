# Build size and installation

## Problem and design reasoning

Players can have enough space for the finished game yet fail an install because the platform needs temporary staging space. Measure compressed download bytes, installed bytes, peak disk use during installation, and time to first playable interaction separately. This guide covers first-install storage and delivery. [Release engineering](release-engineering.md) covers promotion and rollback, and [Downloadable content](downloadable-content.md) covers ownership and optional add-ons.

Bungie documented that Destiny 2's old patch format made it difficult to remove assets superseded by later releases. Cleaning those assets out required a full download for Beyond Light, with preload offered to give players more time. A transferable inference is that retaining every superseded asset to keep incremental downloads small can eventually make storage a bigger player burden than one larger download. [Bungie engineering update](https://www.bungie.net/7/en/News/article/49596)

## Shipped examples

- **Destiny 2: Beyond Light (2020).** Bungie reported a 30–40% size reduction depending on platform from removing unused or replaced assets, optimizing installation, and vaulting some content. Those are combined effects and should be measured separately when evaluating a similar change. [Bungie engineering update](https://www.bungie.net/7/en/News/article/49596)
- **Call of Duty: Modern Warfare II (2022).** Chris Fowler explained how selected high-resolution texture parts moved from the package to on-demand delivery with a bounded local cache. The documented design goals included avoiding visible missing imagery and protecting multiplayer traffic. [Chris Fowler, Extending In-Game Textures Using CDNs, GDC 2023](https://media.gdcvault.com/gdc2023/Slides/Extending%2BIn-Game%2BTextures%2BUsing%2BCDNs_Fowler_Chris.pdf)

## Practical techniques

### Attribute bytes to content and owners

Export an installed package manifest and aggregate sizes by asset class, language, platform variant, and chunk. Compare byte-identical entries with dependencies before removing duplicates. Give regular build comparisons stable labels so that an unexpected size increase can be traced to a content change.

**Check** a clean packaged build before and after removing one suspected duplicate. Compare compressed bytes, installed bytes, and startup. Load every dependent mode and check that a valid asset still resolves.

### Budget the first playable slice

When the platform supports progressive installation, choose an honest playable endpoint and place its dependency closure in the first group. Keep an explicit UI state for activities whose content is pending. Budget first-play time using both download speed and local install work.

**Check** throttle a clean install and launch at the earliest point the platform permits. Complete the advertised activity, request an unready mode, and resume after interrupting the download. Verify stable progress and a clear pending state.

### Make optional detail degrade gracefully

Keep essential gameplay, legible UI, and adequate fallback art in the local package. Bound the cache for streamed presentation assets and deprioritize fetches when gameplay networking or storage is busy.

**Check** clear the cache, disable networking, and load a representative level. Re-enable networking while playing under bandwidth limits. Track texture readiness, disk use, and gameplay latency, and inspect the fallback for missing cues.

### Test peak footprint and recovery

Estimate footprint through download, verification, expansion, and cleanup. Use the resulting peak for support guidance and display a storage error before expensive work if the platform permits an accurate check.

**Check** install with free space just below and just above the asserted requirement. Interrupt once during download and once during installation. Verify resumption or explicit recovery, final installed bytes, and cleanup of partial artifacts.
