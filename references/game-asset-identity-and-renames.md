# Game asset identity and renames

## Problem and design reasoning

Content moves are common during production. A reference that embeds a filename, folder, or child index may break only after an unloaded package, older save, or optional content pack is opened. Separate stable identity from the current path where the project can support it, and manage redirects as compatibility data where old references are name-based.

Santa Monica Studio's *God of War Ragnarök* talk documents replacing fragile scene lookups with an asset database that tracked object GUIDs and supported runtime resolution. The database also let the team locate scripting references and diagnose questionable ones. Epic documents a complementary approach for path-based references: leave an asset redirector at the old location and resave consumers against the new location. The migration workflow below is a recommendation informed by both approaches. [Samuel Sternklar, God of War Ragnarök scripting slides, GDC 2023](https://media.gdcvault.com/gdc2023/Slides/God%2Bof%2BWar_Sternklar_Samuel%2B2023-03-31%2B07.08.17.pdf) · [Epic Games, Asset Redirectors](https://dev.epicgames.com/documentation/unreal-engine/asset-redirectors-in-unreal-engine?lang=en-US)

## Techniques and practical checks

### Preserve identity on move and rename

Give each authored asset an ID stored with its versioned metadata. Derive paths and display names separately. Preserve the ID when moving an asset and issue a new ID when the author duplicates it. Keep explicit replacement mappings when deleting and recreating an asset would otherwise lose its identity.

**Check** rename, move, duplicate, and reimport a test asset in a clean workspace. Verify the original ID resolves after the first two operations and only the intentional duplicate acquires a different identity.

### Scope redirects to supported consumers

Use exact mappings from a legacy path or ID to its intended target. Validate target existence and type, reject cycles, and collapse chains into direct mappings. Document whether each redirect serves a temporary authoring refactor or persisted content from an older release.

**Check** stage an asset rename twice and inject a cycle, missing target, and wrong asset type. Verify the valid historical references resolve to the final asset and each invalid mapping fails diagnostics.

### Validate dependencies without an editor cache

Index known reference sources including scenes, recipe or quest tables, optional packages, and manifests. Include string-based references where they are part of supported authoring. Build from a clean checkout and record unresolved references with their referencing asset, expected type, and old key.

**Check** load a level and an optional pack that were closed when the move occurred. Compare the cooked result with the pre-move fixture and verify that every affected reference is either rewritten or covered by its redirect.

### Retire redirects only after migration

Resave the authoring consumers a refactor can reach and keep compatibility mappings for supported saves or extensions that cannot be bulk-resaved. Avoid reusing legacy names while redirects are still live. Check active usage before deleting a mapping.

**Check** open the oldest supported serialized content and the newest content after the refactor. Verify both bind to the intended asset, and validate the retirement candidate against all supported consumer fixtures.
