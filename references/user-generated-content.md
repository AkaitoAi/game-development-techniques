# User-generated content

## Problem and design reasoning

**Problem.** A creation loads locally but fails after publishing, exceeds a target platform budget, or cannot be reopened after an update. Validate the exact published package as a new player would encounter it.

**Design reasoning.** Epic documents checking memory on cooked content and a separate publishing step. Doom's resource precedence illustrates an inferred complementary lesson that creators benefit from predictable rules for conflicts and dependencies. See the shipped examples below for sources.

## Shipped examples

**Doom (1993).** The released Linux port adds lumps from multiple files to an ordered directory and searches from the end for name lookups. Loading a later WAD can therefore override named resources from the base game. This is a compact extension contract with a defined precedence rule. [id Software, Doom WAD loader](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/w_wad.c)

**Fortnite (2023 onward).** Epic exposes building, playtesting, and publishing of experiences through its creator tools. The documented workflow calculates memory on cooked content and sends publishing through a separate portal. Epic’s Unreal Fest session covers project size, asset reuse, and memory limits for creator content. [Epic Games, Unreal Fest 2023 project optimization talk](https://dev.epicgames.com/community/learning/tutorials/XoB9/fortnite-project-optimization-in-uefn-unreal-fest-2023) · [Epic Games publishing documentation](https://dev.epicgames.com/documentation/en-us/fortnite/publishing-projects-in-unreal-editor-for-fortnite)

## Techniques and practical checks

### Define a content contract

Decide which assets and behaviors creators can customize. Give content durable identifiers and manifests with supported versions and dependencies. Use namespacing for additions and explicit declarations for overrides. Resolve dependency order deterministically and report conflicts before loading.

**Check** a missing dependency, a cyclic dependency, two packages that replace the same item, and a game update that renames a base asset. Verify reproducible ordering and author-facing diagnostics that identify the affected assets.

### Validate against the shipping runtime

Run fast editor checks for missing inputs and required metadata. At packaging, validate supported formats, dependency closure, total package size, and platform-specific runtime budgets. Share core validation rules between local previews and publishing.

**Check** a scene that passes on a developer workstation but exceeds the minimum device budget. Report its largest contributors and validate a revised package on hardware. Verify that a passing package preserves its gameplay when played from published files.

### Separate drafts and published revisions

Keep drafts editable and working releases versioned. Publish a new revision only after checks finish, and maintain a previous approved revision if rollback is supported. Keep saved games or in-progress matches tied to the revision they depend on until migration or completion.

**Check** simultaneous draft edits, a failed publish, a rollback, and joining a session after a new revision is released. Confirm the visible revision, expected dependencies, and player state agree.

### Give players and operators clear controls

Show the content creator, revision, applicable age or content labels, and any additional requirements before launch. Provide reporting and delisting flows for hosted public content. Define how unavailable packages appear in favorites, saved games, and multiplayer sessions.

**Check** with public, private, delisted, and incompatible fixtures. Verify players see the expected availability and that the game offers a clear recovery path for content referenced by a save or session.
