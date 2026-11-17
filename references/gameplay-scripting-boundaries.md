# Gameplay scripting boundaries

Place authorable rules in scripts and give scripts a small set of operations that describe what gameplay intends to do. Put ordering, authority, and teardown where the engine can enforce them consistently.

## Problem and design reasoning

Designers need to iterate on encounters and abilities quickly. Scripts can also outlive an encounter, race with another script, or replay visuals and rewards during correction if the execution rules are implicit.

In *Overwatch*, Blizzard's Statescript runs high-level state machines for hero weapons and abilities. Dan Reed's GDC 2017 talk explains the choice to automate prediction and replication behavior so designers could iterate without implementing common networking mechanics for each hero. In *Destiny*, Justin Truman's GDC 2015 talk describes activity scripts and public-area scripts running under different hosts. Bungie restricted which networked objects mission scripts could change in public areas to avoid scripts interfering with each other. A transferable inference is to scope each script's operations to the state its activity owns. [Blizzard, Networking Scripted Weapons and Abilities in Overwatch](https://media.gdcvault.com/gdc2017/Presentations/Reed_Dan_NetworkingScriptedWeapons.pdf) · [Bungie, Shared World Shooter: Destiny's Networked Mission Architecture](https://media.gdcvault.com/gdc2015/presentations/Truman_Justin_Shared_World_Shooter.pdf)

## Techniques and practical checks

- Give scripts a narrow command interface with explicit preconditions, target scope, and structured results. Keep the authoritative state change in the system that owns the rule. **Check** run two missions against the same shared actor and issue a command against an out-of-scope target. Verify the permitted operation works and the rejected operation produces a useful log without changing state.
- Separate replayable logic from committed effects. Pass in a simulation clock and, where replay must be deterministic, a controlled random stream. Give one-time effects a stable action identity. **Check** predict and replay the same activation after correction. Verify the outcome is consistent and that a reward or one-shot presentation event follows its declared delivery policy.
- Tie waits, subscriptions, and deferred commands to an encounter or ability lifetime. Define what cancellation does before and after commit. **Check** suspend a script on a wait, cancel the activity during a scene transition, then fire its expected event. Verify the old script stays stopped and a new script instance starts cleanly.
- Validate and trace the authored contract. Record script and node identity, trigger, tick, target, and command result. Validate renamed operations and unsupported versions before activation. **Check** load an older script fixture, a graph with a missing operation, and an interrupted encounter. Verify the compatible script still works and failures identify the responsible asset and transition.

See [Gameplay messaging](gameplay-messaging.md) for event ordering and [Gameplay state machines](gameplay-state-machines.md) for transitions.
