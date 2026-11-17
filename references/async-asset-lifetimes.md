# Asynchronous asset lifetimes

## Problem and design reasoning

A request may outlive the object that issued it, while a shared dependency can outlive several of its consumers. An asset can finish loading successfully and still be unsafe to attach to a scene that has already closed. In *Destiny*, Bungie developed resource tracking to validate data lifecycles in its multithreaded engine. For *Destiny 2*, Brandon Moro described splitting an overly connected asset build graph into per-asset graphs and allowing reference fixup at load time to improve content iteration. The runtime ownership rules below are a design inference from those constraints. [Bungie, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-) · [Brandon Moro, Bungie's Asset Pipeline, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Moro_Brandon_Bungies_Asset_Pipeline.pdf)

## Techniques and practical checks

### Give each consumer an explicit lease

Deduplicate in-flight work by asset and version. Give each requester a separate lease, and track child dependencies until the last live lease releases them. Distinguish residency retained by the cache from residency required by a live consumer.

**Check** load the same texture through two different materials. Release one material early and verify the other still renders correctly. Then release both and confirm that cache eviction frees the dependency.

### Treat cancellation as withdrawing interest

Remove a cancelled requester from delivery. If the underlying operation is shared or cannot be interrupted, allow it to finish and dispose of resources that nobody owns. Validate the owner's generation before attaching a result so a pooled or recreated owner cannot receive an earlier request.

**Check** delay a load, destroy its requester, and reuse the same entity slot for another asset. Complete the old request last. Ensure it changes no live game object and releases its lease.

### Publish only fully usable results

Separate reading bytes, decoding, integration on the owning thread, and readiness. Validate required dependencies before publication. Complete each acquisition once with readiness, a documented fallback, or a failure the caller can retry. Release partial dependencies on failure.

**Check** inject a failed dependency after one sibling succeeds. Verify the consumer sees only the intended fallback or failure. Retry after restoring the dependency and confirm intermediate residency returns to baseline.

### Retire resources after the last in-flight use

Drop gameplay ownership when consumers finish and defer actual GPU or audio release until submitted work no longer references the resource. Record acquisition, completion, and release with the asset and owner IDs so ownership leaks can be distinguished from deliberate caching.

**Check** repeatedly switch scenes while queued frames still use old assets. Verify there are no invalid resource accesses and that live leases and resident bytes converge after pending work completes.

For choosing what to load and when as the player moves, see [World streaming](world-streaming.md).
