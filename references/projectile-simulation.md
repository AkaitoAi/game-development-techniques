# Projectile simulation

## Problem and design reasoning

A moving game projectile can cross a thin obstacle between updates, trigger its result twice, or disagree with its visual trail. In *Quake III Arena*, the released `G_RunMissile` code evaluates the projectile trajectory, traces its collision bounds from the previous position to the intended position, and resolves an impact before continuing. The impact path changes the entity's state and creates an event. This makes movement, contact, and retirement part of one ordered update. [id Software, released missile code](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/g_missile.c)

Derek Mattson's GDC 2024 talk on *The Last of Us Part II* describes addressing camera-versus-character perception, hit detection, and input delay, with in-engine debugging used to verify solutions. The talk supports testing what the player sees against what the simulation resolves. The recipes below are portable recommendations and do not assert that Naughty Dog used these particular data structures or collision algorithms. [Naughty Dog, A Second Shot, GDC 2024](https://gdcvault.com/play/1034314/A-Second-Shot-Improving-Ranged)

## Techniques and practical checks

### Sweep the simulated path

Store previous and current simulation positions. Sweep a collision shape over the displacement, resolve the earliest eligible impact, and specify whether any remaining time is used for a bounce or piercing continuation. Bound the number of contacts processed per tick. For curved trajectories, choose substeps or a conservative path approximation against the supported speed and curvature range.

**Check** a stationary thin wall, a crossing target, a curved path near a ceiling, and a deliberate hitch. Test at the maximum supported speed and verify a consistent hit result at supported simulation rates.

### Define impact ownership

Give each projectile instance an identity and a lifecycle with an explicit terminal transition. Resolve damage and other authoritative effects once, then emit presentation events carrying the committed result. Record ignored owners, teams, and surfaces through a shared filter policy.

**Check** overlapping colliders belonging to one target, contact on the expiry tick, and a target destroyed during resolution. Verify one terminal result and the intended number of gameplay grants or damage applications.

### Separate simulation from presentation

Render from the simulated position history or an explicitly predicted state. Make the trail and impact effect agree with the resolved contact. In networked play, define how a provisional projectile maps to its confirmed identity and what happens after a rejected activation.

**Check** delayed confirmations, a correction near an obstacle, and a projectile that expires before confirmation. Watch for duplicate visuals, trails passing through impact surfaces, and lingering emitters.

### Bound capacity and clean up reuse

Budget active projectiles, sweep work, and presentation effects separately. If pooling objects, reset hit history, callbacks, owner references, and remaining lifetime. Select an overload policy that preserves essential gameplay.

**Check** repeated bursts above normal capacity and rapid reuse after scene transitions. Record worst-tick query cost and verify that a recycled instance cannot deliver a result from its prior lifetime.
