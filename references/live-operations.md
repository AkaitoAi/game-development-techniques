# Live operations and seasonal events

## Problem and design reasoning

**Problem.** An event ends while players still hold rewards, or one calendar changes while matchmaking or redemption expects another schedule.

**Design reasoning.** CCP kept traders available after Crimson Harvest participation ended. The design inference is that activity, claim, and cleanup windows can each have their own rules when players need time to collect the value they earned.

## Shipped examples

**Rocket League (2019).** Psyonix ran Radical Summer as a themed in-game event and later recapped it separately from the competitive season. One game can run several content calendars with different player expectations and handoff dates. [Psyonix's 2019 retrospective](https://www.rocketleague.com/news/community-spotlight--best-of-2019)

**EVE Online (2026).** CCP ended Crimson Harvest on November 3 and kept event-item traders available until November 11. Decoupling participation from redemption gives players a chance to spend their rewards after the activity closes. [CCP patch notes, November 3](https://www.eveonline.com/news/view/patch-notes-version-24-01)

**Fortnite, Matsuken Samba.** Alche's Taiki Kawa presented the studio's Fortnite concert at Unreal Fest Bali. A scheduled performance has different delivery needs from a multiweek event, including synchronized starts and recovery for players who join in progress. These are design implications of the case. [Taiki Kawa, Unreal Fest Bali](https://dev.epicgames.com/community/learning/talks-and-demos/mmkk/the-art-of-real-time-concerts-building-a-fortnite-ugc-event-with-uefn?locale=es-es)

## Techniques and practical checks

### Define an authoritative calendar

Give each event an ID, schedule revision, and distinct windows for entry, earning, and redemption. Evaluate transitions against an authoritative clock. Display the equivalent local time in the interface. Specify what happens to a match already running when its entry window closes.

**Check** the moment before, at, and after every boundary. Include a wrong local clock, daylight-saving change, and a reconnect from a different time zone. Confirm the client explanation and server decision agree.

### Make reward claims retryable

Tie a grant to the player, event, and qualifying milestone. Store the grant receipt with the award, so the same request can return the existing outcome. Keep a mapping for players who earned progress before an event revision changed its milestone rules.

**Check** a double click, concurrent claims, and a connection drop immediately after the grant commits. Verify one receipt and one balance change. Test a valid claim for an earlier revision during the redemption window.

### Rehearse staggered delivery

Keep activation separate from package delivery where the game supports it. Stage content and validate eligibility before opening it to everyone. Define how the team can pause entry while preserving existing play sessions and rewards. Give support staff the event ID, revision, and status they need to diagnose player reports.

**Check** an old client, a missing localized asset, and a spike of simultaneous arrivals. Pause entry during rehearsal and verify that the base game stays playable, active sessions follow policy, and grants remain auditable.

### Design the end of the event

Decide what happens to reward balances, incomplete objectives, inventory items, map changes, and communication after close. Record the end-state rule with the event revision. If the event returns, assign a new run ID and define which items or progress carry over.

**Check** expiration for an offline player, a return during the grace period, and another return during the next event. Confirm that the player sees the correct balance and can recover from a partially completed closeout.
