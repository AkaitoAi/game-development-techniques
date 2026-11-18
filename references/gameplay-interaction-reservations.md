# Gameplay interaction reservations

## Problem and design reasoning

Several actors can select the same chair, cover point, ladder, or interaction slot before any reaches it. A valid destination alone does not establish who may use it. Valve's released Half-Life 2 act-busy behavior locks a hint node before committing to use, unlocks an old node when switching, and releases the hint when busy behavior ends. Ownership therefore spans approach and execution, and cleanup belongs to the behavior's lifecycle. [Valve Source SDK, act-busy behavior](https://github.com/ValveSoftware/source-sdk-2013/blob/master/src/game/server/hl2/ai_behavior_actbusy.cpp)

Jeff Orkin's GDC 2006 F.E.A.R. paper explains that its UseSmartObject state obtains animation selection from an object in the game database. This documents data-driven execution context. An exclusive reservation token and timeout are recommended below as a transferable coordination protocol, rather than an attributed F.E.A.R. implementation. [Monolith, Three States and a Plan, GDC 2006](https://pages.cs.wisc.edu/~dyer/cs540/handouts/gdc2006_orkin_jeff_fear.pdf)

## Techniques and practical checks

### Acquire before committing

Represent each exclusive slot with a resource ID, owner handle, and generation. Acquire through one authoritative operation that checks eligibility and availability together. Return a token bound to that owner and generation. Keep discovery separate from acquiring the slot.

**Check** issue two acquisitions in the same tick. Verify one winner for a one-capacity slot, a useful failure for the other actor, and no transition into use without a current token.

### Give approach and use distinct states

Track reserved, approaching, executing, and released states. Define how long approach can continue, which changes require revalidation, and when the actor can abandon the attempt. A capacity limit can allow several independent slots on one object.

**Check** block the route after acquisition, move the object, and invalidate eligibility before arrival. Verify timely release and a bounded retry or alternative choice.

### Release through one idempotent path

Release on success, cancellation, actor removal, resource removal, and session teardown. Reject stale tokens after resource reuse. If using expiry, define which clock drives it and how execution renews a legitimate lease.

**Check** cancel twice, destroy the owner during use, and reuse a pooled resource while an old completion callback remains queued. Confirm the callback cannot release the new owner's reservation.

### Expose contention and fairness

Record acquisition, renewal, failure reason, and release with tick and actor identity. Use a documented queue or selection policy when waiting is appropriate. Keep waits bounded and provide fallback behavior for permanently occupied resources.

**Check** send a group through a narrow interaction point and interrupt the current user. Measure longest wait, throughput, orphaned tokens, and whether every eligible actor eventually proceeds under the intended policy.
