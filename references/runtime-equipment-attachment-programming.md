# Runtime equipment attachment programming

## Problem and design reasoning

An equipped object participates in inventory, animation, rendering, physics, and sometimes networking. Parenting its mesh alone leaves those systems free to disagree about who owns the item or which pose it should follow. The released *Quake III Arena* client code places an equipped model on `tag_weapon`, then attaches a rotating part to a tag on that model. Its comments also identify first-person and world-model calls, with sound and light effects restricted to the intended representation. This is direct evidence that attachment transforms and effect ownership need separate decisions. [id Software, equipped model construction](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_weapons.c)

Naughty Dog's *Uncharted 2* character pipeline describes maintaining one skeleton across outfits using referenced authoring assets. A stable skeleton helps related content share attachment expectations. The equip transaction and generation checks below are portable recommendations, beyond the claims established by the talk. [Richard Diamant and Judd Simantov, Uncharted 2 Character Pipeline, GDC 2010](https://www.gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An)

## Techniques and practical checks

- Define an attachment contract with item identity, owner identity, socket name, local offset, scale policy, and representation. Resolve the socket against the current rig and specify what happens if it is absent. **Check** equip the same object on two supported body variants and one deliberately incompatible rig. Verify pose alignment and an explicit failure or approved fallback.
- Commit equipment state and presentation coherently. Prepare assets, validate eligibility, then publish the chosen item and attach its presentation in a controlled phase. Stamp asynchronous work with the equipment request and owner generation. **Check** request A, request B, then deliver A's asset completion last. Confirm B remains equipped and A releases its prepared resources.
- Give each subsystem explicit ownership during attach and detach. Decide whether the item inherits animation motion or runs as a dynamic body, and define which colliders and effects remain active in each state. **Check** drop an item from a moving actor, re-equip it, and switch camera representation. Verify the expected inherited velocity, collider state, and one logical effect.
- Retire attachments when their owner or rig changes. Remove subscriptions, cancel pending work, and resolve sockets again after a compatible rig replacement. **Check** destroy or pool the actor during equip, reuse its slot, and release delayed callbacks. Confirm the replacement receives no stale attachment or sound.

See [Game transform hierarchies](game-transform-hierarchies.md) for transform composition and [Character customization](character-customization.md) for content compatibility.
