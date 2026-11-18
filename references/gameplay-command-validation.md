# Gameplay command validation

## Problem and design reasoning

The same gameplay action may be requested by a button, AI, script, saved input, or network message. A check in one caller leaves other entry paths with different rules. The released *Quake III Arena* command dispatcher rejects commands before a client is fully in game and limits command handling during intermission. Its weapon simulation also checks player state, weapon timing, ownership, and ammunition while resolving input. These are concrete examples of state checks at the code that handles an action. [id Software, command dispatch](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/g_cmds.c) · [id Software, shared weapon simulation](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/bg_pmove.c)

Blizzard's shipped *Overwatch* uses Statescript for networked weapon and ability logic, with responsiveness, replication, and implementation ease among the concerns discussed in Dan Reed's talk. A portable inference is to make one authoritative validation contract available to every action source and distinguish its result from an optimistic preview. The envelope and rejection policies below are recommendations. [Dan Reed, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://gdcvault.com/play/1024653/Networking-Scripted-Weapons-and-Abilities)

## Techniques and practical checks

### Validate the command envelope before resolving references

Define the command type, issuer, action instance or request ID, target identity, and relevant tick or revision. Check finite numeric values, bounded indices, recognized action identifiers, and resolvable owner and target generations before accessing arrays or mutable objects. Keep malformed requests distinct from valid requests whose gameplay preconditions fail.

**Check** invoke the actual command entry point with a missing target, recycled target identity, negative and excessive indices, and nonfinite numeric input. Verify no gameplay mutation or success event occurs and that rejection identifies the failed category.

### Recheck gameplay conditions at commitment

Evaluate ownership, resources, range, cooldown, phase, and any required reservation against the authoritative state at commit. A UI preview can use the same rule implementation where practical, while the command performs the final check after queued work or delays.

**Check** obtain a valid preview, then spend the resource, move the target, or change the action phase before submission. Submit through UI and AI callers. Compare the accepted state transition or rejection across both paths.

### Give repeated requests explicit semantics

Decide when a command ID denotes a retry of one request and when a new ID denotes a fresh intended action. Retain the terminal result for the required retry window. A rejected attempt can be retried with a new identity when the caller deliberately reevaluates changed conditions.

**Check** deliver a successful request twice after losing its response, then submit a new request with the same action parameters. Verify the retry returns the prior result and the fresh request undergoes current eligibility checks.

### Verify rejection as an observable outcome

Return a bounded reason and the state revision needed by the caller to refresh its preview. Emit success feedback only from the accepted result. In tests, capture the relevant state before rejected commands and compare it afterward, including inventory, cooldowns, reservations, and emitted events.

**Check** run a table of valid, invalid, expired, and duplicate requests through the existing command. Assert exact accepted deltas and unchanged state on rejection, then repeat the fixture through the normal gameplay entry flow.

See [Gameplay invariants and validation](gameplay-invariants-and-validation.md) for rules across transitions and [Gameplay transaction boundaries](gameplay-transaction-boundaries.md) for committing related changes.
