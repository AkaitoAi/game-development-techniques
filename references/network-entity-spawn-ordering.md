# Network entity spawn ordering

## Problem and design reasoning

A client can receive an action referencing an actor before that actor's replica, parent, or content is ready. Reliable delivery alone does not establish application readiness. In *Assassin's Creed Unity*, replicated variable state is applied before the replica enters the world. The talk also describes retaining a replicated call when an entity cannot yet execute it, then trying again when ready. These choices address asynchronous construction and joining an ongoing session. [Charles Lefebvre, Networking Gameplay and AI in Assassin's Creed Unity, GDC 2015, slides 14 and 20–22](https://media.gdcvault.com/gdc2015/presentations/Lefebvre_Charles_NetworkingGameplayAndAI.pdf)

The released *Quake III Arena* client parses entity baselines into its game state and reconstructs snapshot entities from baselines or prior state. This is a different implementation of the requirement to establish decoding context before using updates. The readiness queues and generation protocol below are portable recommendations derived from these ordering problems. [id Software, client game-state and snapshot parsing](https://github.com/id-Software/Quake-III-Arena/blob/master/code/client/cl_parse.c)

## Techniques and practical checks

### Distinguish receipt from readiness

Track whether a network identity is announced, constructed, initialized from authoritative state, and active. Publish the replica to ordinary gameplay only after required dependencies are available. Give construction failure and cancellation explicit outcomes.

**Check** delay a replica's content load while its initial transform, health, and first animation arrive. Confirm it becomes visible with the intended initial state and never ticks with temporary defaults.

### Resolve referenced actors deliberately

Use network identity plus incarnation or another session-valid generation policy. Defer actions requiring unresolved references only when their semantics remain valid. Bound deferred work by size and age and record why expired work was discarded.

**Check** deliver a child attachment before its parent, then remove and recreate the parent identity before releasing the queue. Confirm the action resolves only to the intended incarnation and leaves no permanently waiting child.

### Handle removal during construction

Keep the ordering information needed to reject a stale create or completion after an authoritative removal. Define how the transport's own ordering interacts with local asynchronous jobs. Avoid relying on packet arrival order to order asset-load completions.

**Check** hold a construction completion, receive removal, and then release the completion. Repeat with duplicate create and remove notifications. Verify no retired replica reappears and all construction resources are released.

### Make late-join installation coherent

Install a baseline at an identified authoritative point, then apply the valid subsequent changes. Define how events older than that baseline are skipped or represented in current state. Log baseline identity, replica generation, and pending dependencies.

**Check** join while an actor is created, attached, moved, and removed in quick succession. Replay a recorded network trace with varied local load delays. Compare the final replica graph and deferred queue against the authoritative result.

See [Gameplay spawn lifecycle programming](gameplay-spawn-lifecycle-programming.md) for logical requests and [Network message schema evolution](network-message-schema-evolution.md) for protocol compatibility.
