# Asset source control

## Problem and design reasoning

**Problem.** Artists wait for full syncs or overwrite binary work, and a clean revision cannot resolve the same payloads or importer settings.

**Design reasoning.** Epic documented larger asset payloads increasing Perforce sync time. Splitting metadata from payloads reduces transfer costs where the exact payload remains recoverable by identity. Commit export settings and dependency changes with the assets that need them.

## Shipped examples

- **Saints Row (2006).** Volition technical artist Adam Pletcher's GDC 2012 retrospective describes the team's first vehicle-export pipeline and how a technical artist took ownership of the exporter interface while coordinating its parameter contract with a programmer. His broader talk also covers using source control to let technical artists iterate on pipeline features independently. The vehicle story illustrates why the export tools and their settings need to evolve alongside authored files. [Adam Pletcher, GDC 2012 technical artist bootcamp](https://media.gdcvault.com/gdc2012/slides/1_Visual%20Arts%20Track/Pletcher_Adam_Technical_Artist_Bootcamp.pdf)
- **Fortnite Battle Royale Chapter 4 (2022).** Epic reports that larger asset payloads increased the time spent syncing content from Perforce. Virtual Assets split structured metadata from large payloads such as texture pixels, so workflows could avoid downloading some data until needed. The lesson applies to any system that can retrieve immutable bulk data by identity. The asset revision still needs to identify the exact payload. [Epic Games, Chapter 4 production write-up](https://www.unrealengine.com/blog/battle-testing-unreal-engine-5-1-s-new-features-on-fortnite-battle-royale-chapter-4)

## Techniques

### Set a versioning policy by file role

Identify authoritative source art, engine-facing assets, import settings, stable identifiers, generated intermediates, and delivery artifacts. Version what a colleague needs to reproduce an asset or edit it later. Keep rebuildable intermediates in a cache when the inputs and toolchain can actually reproduce them. Where payloads live externally, publish their immutable identifiers with the tracked metadata.

**Check** by obtaining a tagged revision in a clean workspace with empty caches. Reopen and rebuild a representative scene, texture, and rig. Detect missing source files, unresolved pointers, differing import options, and orphaned metadata.

### Match editorial granularity to the file format

Choose an exclusive checkout for binary or otherwise nonmergeable assets where conflicting edits would lose work. Split frequently shared maps into independently editable units with explicit ownership. Use semantic merge on supported structured files and check the merged output in the editor. Vikram Saran's GDC talk uses a fictional project to illustrate locking and shows how lock scope affects branch integration. [Modern Git for Game Development, GDC 2024](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/Saran_Vikram_Scaling_Git_Workflows.pdf)

**Check** two writers editing the same binary, then two editing separate pieces of a map. Repeat with an offline change and branch integration. Verify that conflicts are visible before publish and that a lock release cannot discard someone else's changes silently.

### Submit dependency changes together

Treat asset creation, renames, deletes, import settings, and reference updates as one reviewable change. Prefer stable identifiers across moves where the editor supports them. Review binary changes with previews or semantic summaries and flag unexpected dependency expansion.

**Check** a rename and a deletion from a clean checkout of the submitted change. Open a dependent scene and run an asset-reference scan. Revert the change in a test branch and confirm that the links and original payloads return together.

### Make large revisions practical to retrieve

Fetch project partitions and payloads on demand where possible. Preserve shipped revision references and their bulk objects through the promised recovery window. Measure representative cold sync, warm sync, and first-open times to catch cost shifted from download to editor use.

**Check** a developer checkout and a build checkout with no local object cache. Simulate an unavailable bulk store and verify clear errors and recovery on retry. Restore a historical release tag and confirm it still resolves the intended assets.
