# Gameplay save dirty tracking

## Problem and design reasoning

A background save can finish after gameplay has made another change. Clearing a single dirty flag on completion can then erase the new save obligation. Tracking that obligation is a different problem from producing a coherent snapshot or committing the file. In shipped *Factorio*, Wube defines save/load correctness as preserving observable behavior and documents the complication of object references and migrations. It provides a useful correctness target, while the dirty-revision protocol below is a portable recommendation. [Wube, Deterministic save/load in Factorio](https://www.factorio.com/blog/post/fff-270)

Barry Genova's *Destiny* presentation describes job graphs with validation of when reads, writes, creation, and destruction may occur. This supports making capture and completion ownership explicit across asynchronous work. The talk does not establish that Destiny used the save-revision scheme proposed here. [Barry Genova, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-Destiny)

## Techniques and practical checks

### Track mutation and persisted revision separately

Advance a revision when accepted gameplay changes persistable state. Capture both the state and its revision at a defined boundary. On successful durable commit, acknowledge that captured revision. If the current revision is newer, leave saving pending. Compare revisions within the same session or use an explicit epoch across reloads.

**Check** capture revision 10, mutate to 11 while serialization is blocked, and complete the revision-10 save. Assert persisted revision is 10, current revision is 11, and another save remains necessary.

### Put dirty marking on the accepted mutation path

Have commands report persistent change after validating eligibility and applying the mutation. Include less obvious paths such as inventory transfers, migration corrections, and object deletion. Keep derived presentation caches outside the persistent-change contract unless the save format actually owns them.

**Check** exercise each supported mutation through its normal caller, then compare the dirty revision and loaded result. Reject an invalid reward request and verify currency, progress, and revision remain unchanged. Restore a deleted object from an older save to verify deletion is represented.

### Define independent domains only where useful

Use separate revisions for save domains that can commit independently, such as options and world progression. When an operation moves state across domains, capture a transaction or use one commit group so a crash cannot preserve only one side of the transfer. Profile before introducing fine-grained per-object tracking.

**Check** change settings during a world autosave and inject failure in a cross-domain inventory transfer. Reload the last complete commit and verify the selected consistency policy, with no duplicated item or lost ownership.

### Keep failure and retry obligations visible

Retain the pending revision after serialization failure, storage exhaustion, or cancellation. Attach completion to the capture's session generation so an old save cannot acknowledge a replacement world. Report the last committed revision separately from queued and in-flight work.

**Check** fail a write, retry, then unload the world while another save is pending. Deliver its delayed completion after a new world loads. Verify the new world's dirty state is untouched and each failed operation has a visible recovery path.

See [Save and load threading](save-load-threading.md) for snapshot capture and [Save commit protocols](game-save-commit-protocols.md) for durable publication.
