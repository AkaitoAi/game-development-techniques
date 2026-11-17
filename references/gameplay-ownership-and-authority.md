# Gameplay ownership and authority

## Problem and design reasoning

When two players grab the same object, an actor changes possession, or a host disappears mid-mission, several systems may have a plausible claim to update the same state. Track which component owns the lifetime and storage, which actor can request a change, and which simulation or service commits the result. Prediction and visual presentation can move ahead of a commit under a defined reconciliation policy.

Bungie's *Destiny* needed responsive actions, drop-in cooperative missions, and interactions with strangers. Justin Truman's GDC 2015 talk describes the network architecture built around those goals. Bungie later explained that *Destiny 2* moved mission and physics hosting to its data centers while retaining player authority over their own movement and abilities. The transferable lesson is to assign authority to a specific class of state and document the boundary between those classes. [Justin Truman, Shared World Shooter: Destiny's Networked Mission Architecture, GDC 2015](https://www.gdcvault.com/play/1022246/Shared-World-Shooter-Destiny-s) · [Bungie on Destiny 2 networking](https://www.bungie.net/7/en/News/Article/45919)

*Rocket League* makes a different choice for shared match physics. Lead gameplay engineer Jared Cone describes server authority with client prediction and correction, plus buffered player input on the server. For a ball that two players can hit on the same tick, publishing a common outcome supports consistent match state. [Jared Cone, It IS Rocket Science! The Physics of Rocket League Detailed, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/Cone_Jared_It_Is_Rocket.pdf)

## Techniques and practical checks

### Assign authority by operation

Record the owner and lifetime of every shared record. Specify which actor can request an operation, which system validates and commits it, and which views react after commit. For a vehicle, the seat owner can request throttle while the match simulation commits movement and scoring.

**Check** an ordinary request, a request after a seat change, and a request at a match boundary. Confirm each operation reaches its designated decision point and that observers agree on the committed outcome.

### Make handoffs explicit

Transfer control at a declared tick and track a control generation alongside stable entity identity. Define which pending requests survive a handoff and when prediction must reconcile. Keep persistent ownership intact when temporary control changes.

**Check** disconnect the driver just before another player takes control in a simulated two-client session. Deliver an older buffered input after the transfer. Confirm the new owner can act and obsolete input cannot steer the vehicle.

### Resolve contested actions in one place

Treat a pickup or interaction as a request to the record owner. Resolve concurrent requests by a documented ordering rule. Emit durable rewards from the committed event and use a stable operation identifier to handle retry delivery.

**Check** send two valid pickup requests for one item in the same tick. Repeat with arrival order reversed and retry the accepted request. Confirm the configured tie rule, a single award, and an appropriate response for each player.

### Define recovery by state type

Decide how each record behaves when its authority disappears. Durable objectives may restore from committed state, while ephemeral effects can expire. Reconnect using a current authority snapshot and refresh predictions based on acknowledged input.

**Check** interrupt a session between request and commit, then between commit and replication. Reconnect and join late in each case. Inspect objective, inventory, and presentation state for missing or repeated effects.

For runtime capture and restoration, see [Gameplay serialization and snapshots](gameplay-serialization-and-snapshots.md). For repeated simulation effects, see [Resimulation-safe gameplay events](resimulation-safe-gameplay-events.md).
