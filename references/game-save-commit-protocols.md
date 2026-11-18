# Game save commit protocols

## Problem and design reasoning

A save can contain a coherent snapshot yet fail while replacing the previous disk image. Snapshot capture, persistence, and publication therefore need distinct completion states. Wube documents using a forked process to save *Factorio* on macOS and Linux while play continues, with additional memory cost and unresolved edge cases. This is evidence for separating snapshot work from the interactive loop. It does not establish Factorio's disk publication protocol. [Wube, Friday Facts 408](https://www.factorio.com/blog/post/fff-408)

Alexander Silkin's GDC 2024 session describes Survios practices evolved through shipped titles including *Raw Data* and *Creed: Rise to Glory*. Its save-system material provides production context for collecting persistent state from gameplay components. The commit procedure below is an engineering recommendation rather than a claim about those titles' storage implementation. [Survios, Techniques for Efficient Development of VR Games, GDC 2024](https://gdcvault.com/play/1034834/Future-Realities-Summit-Techniques-for)

Microsoft's Game Save API documents transactional updates to one container, which keeps related writes consistent. Platform transaction guarantees, filesystem replacement guarantees, and power-loss durability are separate properties to verify on the target platform. [Microsoft, XGameSaveSubmitUpdate](https://learn.microsoft.com/en-us/gaming/gdk/docs/reference/system/xgamesave/functions/xgamesavesubmitupdate)

## Techniques and practical checks

- **Capture one generation.** Give every snapshot a generation ID and freeze or copy all state required for that generation at a simulation boundary. Keep writing from that snapshot while gameplay advances. **Check** award currency while an inventory snapshot is being captured. Verify that the loaded result corresponds to one complete generation and preserves the inventory–currency invariant.
- **Publish a complete save.** Stage a generation, validate its schema and integrity, then use the platform's supported transaction or documented replacement protocol. Group dependent records within the same atomic boundary where possible. Preserve a prior valid generation according to the game's recovery policy. **Check** inject termination before staging, during writing, and immediately before and after publication. Restart and verify that recovery selects a complete generation.
- **Serialize competing writers.** Route autosave and manual save requests through a per-slot owner. Coalesce requests only when the newer snapshot supersedes the older one, and prevent late completion from publishing an older generation over a newer one. **Check** delay generation A while B is requested, then allow completions in reverse order. Confirm the recovered slot follows the declared ordering policy.
- **Distinguish saved from queued.** Report success after the persistence operation reaches the guarantee promised to the player. Treat cloud upload as another state when applicable. Surface quota or storage failure with a retry path that retains the last valid save. **Check** full storage, permission failure, and a failed cloud upload after successful local commit. Verify the displayed status matches the durable local and remote generations.

For format changes, read [Save schema migrations](save-schema-migrations.md). For multi-device conflicts, read [Cloud save sync](cloud-save-sync.md).
