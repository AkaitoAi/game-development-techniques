# Save schema migrations

## Problem and design reasoning

A save can decode successfully while representing a world the new build cannot run. Renamed items, removed content, and new progression rules can invalidate references or change what a field means. Wube's *Factorio* postmortem describes keeping prototype ID mappings and handling historical map versions, mod changes, and invalid saves. Mojang documents composable rewrite rules for converting *Minecraft: Java Edition* data between versions. Alexander Silkin's GDC presentation on VR production at Survios also explains why accurate debug-save snapshots are valuable late in a milestone and easy to invalidate during active development. The implementation inference is to keep stable released saves as migration fixtures and refresh short-lived debug saves separately. [Wube, Factorio Save/Load Overview](https://www.factorio.com/blog/post/fff-270) · [Mojang, DataFixerUpper](https://github.com/Mojang/DataFixerUpper) · [Alexander Silkin, Techniques for Efficient Development of VR Games Using Unreal Engine 5, GDC 2024](https://media.gdcvault.com/gdc2024/Slides/GDC%2Bslide%2Bpresentations/Silkin_Alexander_Techniques%2Bfor%2BEfficient.pdf)

## Techniques and practical checks

### Give the persisted format a version

Store a schema version in a small header and keep it distinct from the display or patch version. Decode into save data before constructing runtime objects. Define which versions are accepted and reject unsupported newer saves without writing over them.

**Check** load the oldest supported fixture, the current format, and a fixture marked with a future schema. Verify only supported versions load and the future save remains byte-for-byte intact.

### Apply ordered migrations based on the saved version

Use a defined sequence such as v1 to v2 to v3. Keep transitions narrowly scoped and deterministic where possible. Run structural changes before cross-record fixups. Rebuild derived fields from authoritative data when the new rules allow it.

**Check** migrate v1 directly with the current loader and compare the result with independently staged v1-to-v2 and v2-to-v3 outputs. Reopen the resulting v3 save and verify grants, unlocks, and counters remain stable.

### Rebind content references explicitly

Maintain stable IDs or a documented rename map. Define whether a removed item becomes a replacement, a placeholder, a refund, or a recoverable missing-content error. Rebind cross-object references after the necessary records exist. Validate that key progression relationships still hold.

**Check** upgrade saves containing renamed objects, an obsolete inventory item, and an objective pointing at absent content. Confirm each follows its declared policy and the remaining objectives can still complete.

### Commit only after semantic validation

Validate the upgraded data, write a separate candidate, and use the platform's supported commit operation to replace the live save only when the candidate is durable. Preserve a recovery path to the original. Make migrations operate on isolated data so failed retries do not issue extra rewards or external writes.

**Check** interrupt migration before validation, during candidate writing, and at commit. Confirm the original or the fully upgraded file is readable and a retry creates no duplicate grants. Retain a released-build fixture for every supported migration entry point.

For save slots, persistence boundaries, and save timing, see [Saves and persistent world state](save-systems.md).
