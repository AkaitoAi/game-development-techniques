# Multiplayer session lifecycle

## Problem and design reasoning

**Problem.** A leaving host freezes everyone, a reconnect duplicates a participant, or a live session gets cut off by infrastructure shutdown.

**Design reasoning.** Bungie moved hosting responsibilities into data centers for Destiny 2. The design inference is to decide who owns each piece of session state and rehearse both planned handoff and abrupt loss.

## Shipped examples

**Halo: Reach (2010) and Destiny (2014).** Bungie's Justin Truman contrasted the abrupt black screens typical of Halo: Reach host transfers with Destiny's work on host handoffs and ungraceful migrations. Destiny combined peer-hosted responsibilities with server infrastructure to support shared spaces and drop-in missions. [Justin Truman, Bungie, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Truman_Justin_Shared_World_Shooter.pdf)

**Destiny 2 (2017).** Bungie moved both its mission and physics hosting roles to data centers, reducing dependence on an individual participant remaining connected in activities such as raids. This illustrates that disconnect behavior depends on which role the leaving participant owns. [Bungie networking explanation](https://www.bungie.net/7/en/News/article/45919/7_This-Week-At-Bungie--05252017)

**Fortnite Creative (2024).** Epic documented changes to published island and edit-session shutdown behavior. Servers began responding to their host's planned shutdown timing. A session's lifetime needs to account for the infrastructure already hosting it. [Epic's 33.00 release notes](https://dev.epicgames.com/documentation/en-us/fortnite/33-00-fortnite-ecosystem-updates-and-release-notes-in-creative-and-unreal-editor-for-fortnite)

## Techniques and practical checks

### Give each stage an explicit owner

Model stages such as waiting, starting, active, ending, and closed. Track stable identities for the party, running instance, and each player so a connection can change without creating new membership. Assign one authority for each transition and make retries safe to process.

**Check** simultaneous start and leave, duplicate ready messages, and cancellations arriving during startup. Trace the resulting state and confirm one session starts, aborted reservations clear, and every participant sees the same outcome.

### Treat rejoin as state restoration

Define a grace period and what happens to a disconnected participant's slot and avatar. On rejoin, restore membership, clear stale transient client state, and wait for a complete baseline before allowing actions. Distinguish temporary loss from a voluntary departure.

**Check** a dropped connection during startup, a disconnect that crosses a round boundary, and a return just before and after expiry. Confirm consistent team, score, character ownership, and UI. Check that two simultaneous reconnect attempts result in one active membership.

### Plan for host loss

Choose a recovery model that fits the game. A dedicated instance can keep running after the party leader leaves. A peer-hosted game may elect a successor, restore a transferable snapshot, and advance the session's authority generation. Where recovery is unsupported, return the party to a defined state and report the outcome.

**Check** a clean transfer, sudden host termination, and a second failure during recovery. Verify that one authority advances the match, state loss follows the documented policy, and players receive a clear status during the interruption.

### Finalize and drain

Close new admissions before stopping an instance. Define when active rounds can finish and when an infrastructure deadline requires termination. Commit results using the identity of the completed match and acknowledge only durable outcomes.

**Check** shutdown during waiting, active play, and result delivery. Retry result submission after interruption and confirm one reward and one result per participant. Inspect orphaned reservations and instances after the drain deadline.
