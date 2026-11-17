# Game object pooling

## Problem and design reasoning

Spawning and destroying short-lived game objects can make update time and allocation volume spike together. Reuse can trade that churn for retained memory, a capacity policy and explicit cleanup. An object pool owns reusable objects and their activation contract. A memory allocator can supply storage independently of that contract.

**Shipped examples.** The released Quake source clears reused server entity records and unlinks freed entities from the world. Its allocation comments explain that rapid reuse of an entity slot may make clients interpolate between two different objects, so the runtime normally defers reuse shortly after release. Insomniac's Joe Valenzuela reported using object pooling to reduce memory volatility in Outernauts. The shared lesson is an inference from those designs. Even with reusable storage, ownership and active identity need separate rules. [id Software's Quake entity code](https://github.com/id-Software/Quake/blob/master/WinQuake/pr_edict.c) · [Joe Valenzuela, Outernauts: From AAA Console to AAA Flash, GDC 2013, slides](https://media.gdcvault.com/gdc2013/slides/822329Valenzuela_Joe_Outernauts_From_AAA.pdf)

## Techniques and practical checks

### Profile a candidate before pooling it

Measure how often objects of one type spawn, how many coexist at peak, and which parts of construction dominate cost. Estimate idle memory and any warmup cost. Choose a type with expensive initialization or high churn and compare a pooled prototype against simple construction under the same scene load.

**Check** a cold start, representative play and a worst-case burst. Compare peak retained memory, loading time and high-percentile frame time. Verify that savings persist after warmup and justify the additional lifetime rules.

### Define the activation and release contract

Activate only after owner, transform, and type-specific state are initialized. When releasing, mark the instance inactive for that lease and remove it from simulation, collision, tick lists and subscriptions before offering it to another caller. Reset mutable state that would otherwise carry forward, including pending timers, cached targets and presentation state.

**Check** reuse an object immediately after it held unusual state, such as an expired timer, non-default velocity, and a subscribed callback. Compare its next activation with a newly created instance and confirm inactive objects cannot affect the world.

### Distinguish identity from storage

Use a lease token or generation-checked handle when async callbacks or other owners may keep references. Advance the generation on reuse and validate it at every delayed completion or release. Replicate separate spawn identity where the same slot can represent different visible objects to a client.

**Check** release instance A, acquire its slot as B, then deliver A's delayed callback and a second return attempt. Verify B stays active, its state is intact, and clients show a clean despawn and respawn even when updates arrive late.

### Bound capacity and teardown

Choose an exhaustion policy by object class. Cosmetic work may be suppressed under load. Gameplay-critical work needs a budget or another behavior that preserves simulation rules. Drain or invalidate outstanding leases when the owning scene or session shuts down, then retire cached objects within that lifetime.

**Check** exhaust the pool during repeatable gameplay and unload the owning scene with live and idle leases. Verify the documented overflow behavior, correct gameplay outcomes, and zero retained references after teardown.
