# Entity identifiers and handles

Use a reference whose lifetime matches the work that will consume it. Frame-local access, a queued callback, and a save-file reference can require different representations even when they refer to the same entity.

## Problem and design reasoning

A delayed action that stores a bare array index can affect an unrelated object after the slot is recycled. id Software documents this problem in shipped *Quake* source. The allocator avoided immediate slot reuse in ordinary play so clients would see removal and creation as separate events and avoid incorrect interpolation and trails. Valve's Source SDK for *Half-Life 2* packs an entry index and a serial number into a handle. An initialized handle and a live target are separate conditions. [id Software edict allocator](https://github.com/id-Software/Quake/blob/master/WinQuake/pr_edict.c#L71-L104) · [Valve Source SDK base handle](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/public/basehandle.h)

Chris Butcher's GDC 2015 talk treats object-system design and data lifetime as architectural concerns drawn from *Destiny*. An engineering inference across these cases is that a lookup contract should express when the identity is valid, where it resolves, and what happens after removal. [Bungie, Lessons from the Core Engine Architecture of Destiny](https://gdcvault.com/play/1022106/Lessons-from-the-Core-Engine)

## Techniques and practical checks

- Pair a reusable slot with its generation and validate both when resolving a runtime handle. Include a world or registry scope where multiple simulations coexist. **Check** destroy an entity, force slot reuse, and process a delayed command containing the old handle. Confirm no state changes on the replacement and that its new handle resolves. Exercise wraparound if the generation field can wrap within a supported session.
- Give durable references a stable identity within a defined save or content namespace. Resolve them to fresh runtime handles after loading. **Check** reorder authored objects, stream the target region out and back in, and load a save referencing a removed target. Verify the surviving IDs resolve and the removed target follows the declared fallback.
- Use a lookup result that distinguishes missing, unloaded, and ready where callers need different recovery behavior. Resolve after crossing an asynchronous boundary, inside the scope that owns mutation. **Check** unload or destroy a target between queueing and consuming a job. Verify cancellation or deferral follows policy and that a later replacement cannot receive the old result.
- Make replication lifetime explicit if IDs can be reused. Reset interpolation and presentation when the replicated instance changes. **Check** deliver spawn, update, and removal events under simulated delay and reordering. Confirm obsolete updates cannot revive a removed instance or move a newly spawned one.
