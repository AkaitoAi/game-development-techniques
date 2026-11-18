# Runtime save slot management

## Problem and design reasoning

A valid save payload can still be written into the wrong slot after a user change, stale menu selection, or delayed completion. Slot management determines which progress record an operation belongs to and how that identity survives UI sorting, asynchronous work, and account changes. The disk commit procedure is a separate concern.

In the released *Quake* source, save and load commands use a named file within the game directory. The save writes a version and descriptive comment, and loading checks the version before reconstructing the map. This shows a concrete separation between the selected save name and the payload describing the session. It also shows why displaying metadata and validating compatibility matter. The profile and generation protocol below is a portable recommendation, rather than an attribution to Quake. [id Software, Quake save/load commands](https://github.com/id-Software/Quake/blob/master/WinQuake/host_cmd.c)

Alexander Silkin's GDC 2024 talk describes Survios practices developed through shipped games including *Raw Data* and *Creed: Rise to Glory*. It provides production context for save-system design as a reusable service. Its session overview does not establish a particular slot ownership or conflict policy, so the following contracts should be tested against the target project's requirements. [Survios, Techniques for Efficient Development of VR Games, GDC 2024](https://gdcvault.com/play/1034834/Future-Realities-Summit-Techniques-for)

## Techniques and practical checks

### Keep stable slot identity separate from display order

Identify a slot by its owning profile and a stable slot ID. Give display names, thumbnails, and sorted menu positions their own metadata. Pass a captured identity to load and save operations rather than consulting the current row selection when work completes. Define how autosave, manual save, and quicksave choose their destination.

**Check** start a load from row two, refresh and reorder the list, then complete the request. Assert that the loaded payload belongs to the originally requested slot. Rename another slot and verify that its stable identity and contents remain intact.

### Serialize mutations within the slot scope

Give create, overwrite, rename, and delete operations explicit ownership and outcomes. Use a slot generation or revision to prevent a completion for a deleted record from recreating it. Decide whether a new write replaces a queued request, waits behind it, or is rejected. Delegate crash-consistent publication to the storage commit layer.

**Check** queue save A, request an overwrite B, and complete A late. Repeat after deleting and recreating a slot under the same display name. Verify the declared ordering policy and that an obsolete generation cannot replace the new record.

### Resolve profile and device conflicts deliberately

Associate queued work with the profile that initiated it. On sign-out, stop new operations and settle or cancel owned work according to the platform contract. Treat a remote conflicting version as a separate candidate until the conflict policy selects it. Keep local availability and cloud synchronization status explicit.

**Check** switch profiles during a save, load, and thumbnail request. Confirm each completion stays with its original owner or reports cancellation. Introduce two valid cloud versions and verify the selected conflict outcome without losing the unselected candidate before policy permits cleanup.

### Publish list metadata from committed records

Use metadata tied to a committed payload generation. Show pending operations as pending and keep a failed overwrite distinguishable from a saved record. Validate compatibility before offering resume. Provide a recovery outcome for missing payloads or unreadable metadata.

**Check** fail an overwrite, remove a thumbnail, and provide an incompatible payload in separate fixtures. Reopen the menu and verify it shows the last valid progress, a usable thumbnail fallback, and the correct load availability.

See [Save commit protocols](game-save-commit-protocols.md) for publication guarantees and [Cloud save sync](cloud-save-sync.md) for multi-device reconciliation.
