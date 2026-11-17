# Mobile runtime downloads and data budgets

## Problem and design reasoning

A small install can still have a long time to first play if the game downloads everything after launch. Fetching every optional asset on demand can cause repeated stalls or unexpected mobile data use. Divide content by when the player needs it and what the game can do if that content is missing. Set distinct budgets for network bytes, temporary storage, cache size, and elapsed time.

Pokémon GO's help center explicitly recommends downloading assets over Wi-Fi to reduce mobile data use while exploring. In a related console case, Sledgehammer measured how much packaged data players accessed and investigated delivering optional textures remotely to reduce installation size. **Design inference.** Keep essential gameplay data local and give the player control over when bulky optional assets arrive.

## Shipped examples

- **Pokémon GO.** The game offers a setting to download available assets in advance alongside its default gradual downloading. A player can prepare over Wi-Fi before going out or attending an event. [Pokémon GO help center](https://scopelyexplore.helpshift.com/hc/en/5-pokemon-go/faq/17-download-all-assets/)
- **Call of Duty: Modern Warfare II.** This console example documents the evolution of on-demand texture streaming across several entries, from a Modern Warfare prototype through Vanguard and Modern Warfare II. The shipped approach separates component textures from equipment objects. This is useful precedent for keeping game objects usable when optional high-resolution content is still arriving. [Chris Fowler, GDC 2023, Extending In-Game Textures Using CDNs for Call of Duty: MWII](https://media.gdcvault.com/gdc2023/Slides/Extending%2BIn-Game%2BTextures%2BUsing%2BCDNs_Fowler_Chris.pdf)

## Implementation checks

1. **Keep a playable core.** Mark which files are required to enter and complete the first session. Stage optional modes, languages, or high-resolution assets independently. **Check** a clean install with the network disabled after authentication or with the content service unavailable. Confirm the supported offline or fallback route and measure time to a playable state.
2. **Make the data decision visible.** Show estimated transfer size before a large request. Respect the player's metered-network choice, and re-evaluate it when the network changes. Separate the transfer estimate from installed size. **Check** Wi-Fi-to-cellular and cellular-to-Wi-Fi transitions, including a metered Wi-Fi connection where supported. Inspect actual bytes and verify that paused work resumes only under the selected policy.
3. **Resume and commit consistently.** Use a versioned manifest and a staging area. Resume verified chunks when supported, validate the final asset set, then make it active. Keep a usable version or fallback until the new set is complete. **Check** force-close during transfer and during installation, then relaunch with low storage or changed content versions. Verify byte reuse, consistent assets, and a clear recovery path.
4. **Measure cold and warm use.** Prioritize requests needed by the current scene, cap competing downloads, and evict optional content within a storage budget. **Check** the same movement route with a cold cache, a warm cache, and an evicted cache. Record network bytes, disk peak, and stalls. Confirm that missing visual upgrades do not affect rule clarity.

For package-level decisions, see [Mobile store delivery](mobile-store-delivery.md). For reconnect and offline progression, see [Mobile offline play and networking](mobile-offline-and-networking.md).
