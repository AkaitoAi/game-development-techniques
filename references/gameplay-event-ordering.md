# Gameplay event ordering

## Problem and design reasoning

Several valid events can contend for the same state in one simulation tick. A shield expiration and an incoming hit produce different results depending on their order. If callback registration, thread completion, or packet arrival decides precedence, players can see inconsistent outcomes and replay tests can diverge.

In *Factorio* 1.1, workers processing independent belt groups could both request a wakeup for the same inserter. Wube explains that activation order also defines update order, so the main thread merges wakeup requests according to a deterministic group order after the workers finish. In his GDC 2017 *Overwatch* talk, Dan Reed describes dropping redundant or out-of-order state packets before applying replicated data or replaying local frames. The two examples address different boundaries. The first fixes precedence among simulation requests. The second orders which revisions the simulation can consume. [Wube, Friday Facts #364](https://www.factorio.com/blog/post/fff-364) · [Dan Reed, Networking Scripted Weapons and Abilities in Overwatch, GDC 2017](https://media.gdcvault.com/gdc2017/Presentations/Reed_Dan_NetworkingScriptedWeapons.pdf)

## Techniques and practical checks

### Define an explicit phase order

Choose phases for collection, resolution, and notification. Write down priorities for conflicts that change the outcome, such as processing expirations before hits or resolving simultaneous hits from the same pre-hit state. Keep the rules in a named policy shared by production code and tests.

**Check** place a shield expiry, a damage request, and a healing request on the same tick. Assert the final state and event sequence for the intended contract, then repeat at one tick earlier and later.

### Merge independent producers deterministically

Collect requests from parallel producers independently. At a barrier, sort conflicting requests by a stable key such as phase, target ID, source ID, and a producer-local sequence. Log the complete ordering key for ambiguous outcomes.

**Check** perturb worker completion order and vary worker count while replaying the same inputs. Compare ordered gameplay outcomes and state digests. Include two producers targeting the same object.

### Choose when reactions can emit more work

Route immediate reactions into a later phase or defer them to the next tick. Define how duplicate intents combine and set a bound on reaction chains where effects can trigger one another.

**Check** exercise a reaction that produces another reaction and two requests with the same intent ID. Confirm the chain terminates and legitimate independent events still apply.

### Handle late input at the simulation boundary

Give external inputs an authoritative tick or sequence. Decide whether an older input can trigger resimulation, is incorporated at a safe boundary, or expires. Emit visible confirmations from the committed outcome.

**Check** feed one trace with delayed, repeated, and reordered external messages. Compare the accepted command stream and committed effects with a reference run using the same lateness policy.

See [Gameplay messaging](gameplay-messaging.md) for routing and [Simulation command buffers](simulation-command-buffers.md) for deferred structural mutations.
