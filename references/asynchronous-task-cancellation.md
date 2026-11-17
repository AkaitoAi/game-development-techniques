# Asynchronous task cancellation

## Problem and design reasoning

Closing a menu, replacing a path request, or leaving a session can invalidate the recipient of background work before the work finishes. In *Destiny*, Bungie used a job graph and resource tracking to validate reads, writes, construction, and destruction against safe points in the frame. The shipped *Destiny* and *Destiny 2* login queues also have a visible ownership boundary: a player who disconnects loses their queue position. These sources support reasoning about ownership and safe publication. The cancellation protocol below is an engineering recommendation. [Chris Butcher, Multithreading the Entire Destiny Engine, GDC 2015](https://www.gdcvault.com/play/1022164/Multithreading-the-Entire-) · [Bungie, Destiny Server Queues](https://help.bungie.net/hc/en-us/articles/360049202011-Destiny-Server-Queues)

Microsoft's GDK documentation separates background work from completion dispatch and explains that cancellation depends on the operation. This makes cancellation intent, worker termination, and safe publication separate decisions in a game task. [Microsoft GDK asynchronous programming model](https://learn.microsoft.com/en-us/gaming/gdk/docs/features/common/async/async-programming-model)

## Implementation and verification

- **Bind work to its owner.** Give a screen, entity, or session a cancellation scope and give replaceable requests an identity. On completion, check the current ownership and request identity on the mutation thread immediately before publishing. Release stale results using their original ownership rules.
  **Check** issue path requests A and B for one actor and deliver B before A. Repeat while replacing the actor in the same slot. Verify that only the current request updates the live actor.
- **Make cancellation cooperative.** Stop queued work where the scheduler allows it and check cancellation at safe chunk boundaries in workers that can stop. Define one terminal result and one cleanup path. Keep callback-owned resources alive until the worker or API has acknowledged that it has finished with them.
  **Check** request cancellation before start, mid-execution, immediately before completion, and twice. Verify one terminal notification and one resource release per accepted request.
- **Define the commit boundary for side effects.** A cancelled client wait can leave a remote reservation or other committed result. Give these operations stable request identifiers and a reconciliation path. Release abandoned reservations or let them expire according to the service contract.
  **Check** let a reservation commit just before the player leaves the queue, then delay delivery of the success response. Confirm the client stays in the correct screen and the orphaned reservation is released or expires.
- **Drain or detach at shutdown.** Stop accepting work for the retiring owner, request cancellation, and keep required cleanup alive through completion. Avoid waiting on the game thread for jobs whose completion must run on that same thread.
  **Check** unload a level with work queued and executing, including a completion that needs the main thread. Verify no deadlock, no update of disposed state, and no outstanding task resources after teardown.
