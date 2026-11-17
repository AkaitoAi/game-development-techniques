# Game mod support

## Problem and design reasoning

Mods extend the shipped game through a supported contract for data, assets, or behavior. When tools or runtime assumptions change during updates, mod authors need predictable extension points and players need actionable recovery. Decide who owns that contract before exposing internal objects. For player creation workflows and publication, see [User-generated content](user-generated-content.md). This guide focuses on runtime integration and compatibility.

## Shipped examples

**The Elder Scrolls III: Morrowind** and **The Elder Scrolls V: Skyrim** shipped with official tooling for mod authors. Bethesda's Joel Burgess discussed how the studio's experience supporting modding influenced tool design and team workflows. The inference for a new project is that tool support has production costs and benefits that extend beyond publication. [Joel Burgess, How Modding Made Bethesda Better, GDC 2015](https://www.gdcvault.com/play/1022107/Level-Design-in-a-Day)

**Factorio** formalizes package identity, versions, dependencies, startup phases, runtime scripting, and migrations. The documented dependency order controls both loading and runtime event delivery. Separating asset and prototype definition from active gameplay gives the game an opportunity to validate content before opening the world. That validation benefit is an inference from the architecture. [Wube, Mod Structure](https://lua-api.factorio.com/latest/auxiliary/mod-structure.html) · [Wube, Data Lifecycle](https://lua-api.factorio.com/latest/auxiliary/data-lifecycle.html)

## Techniques and practical checks

### Publish a bounded extension contract

Start with data schemas and documented hooks for specific use cases. If the game accepts scripts, run them with only the game capabilities they need and bound their runtime work. Define who owns the authoritative state and when hooks can modify it.

**Check** install a minimal data mod and a small supported behavior mod. Confirm each changes only the intended output. Verify that invalid content and unsupported operations produce actionable diagnostics and leave base content playable.

### Resolve dependencies before loading content

Require a stable package identifier, version, supported game versions, and dependency rules. Resolve one ordered package set before touching live game state. Define tie-breaking and conflict handling so two installations can reproduce the same result.

**Check** required and optional dependencies, a dependency cycle, incompatible versions, and two mods claiming the same definition. Compare the resulting load order and error messages on two fresh installations.

### Keep saves and multiplayer explicit about mods

Record the active mod versions and settings in saves or session metadata. Decide when a missing mod blocks loading, when data can survive as inactive state, and when a migration is required. Preserve the prior save until a migration completes.

**Check** load the same save with one exact version, one eligible upgrade, and one missing mod. Confirm the game reports the difference before making irreversible changes. For multiplayer, test an exact match and a mismatched mod manifest.

### Support diagnosis and recovery

Include the package identity and phase in diagnostics. Provide an isolated test profile and a way to recover from failed startup. Let authors exercise a representative validation flow before publishing and provide clear information about supported API changes.

**Check** introduce an invalid asset and an initialization failure in a development fixture. Confirm that the player can identify the mod, reach recovery, restart with a compatible set, and load an unaffected save.
