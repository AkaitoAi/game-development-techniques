# Achievements and leaderboards

## Problem and design reasoning

**Problem.** A rank tells players where they placed but leaves improvement opaque, or an achievement rewards repetition regardless of mastery.

**Design reasoning.** A design inference from Geometry Wars and Mario Kart 8 is to let achievements invite a different play pattern and let ranked records double as demonstrations when replay data is available. Trackmania illustrates comparisons against thresholds, peers, and ghosts serving distinct purposes.

## Shipped examples

**Geometry Wars: Retro Evolved (2005).** A GDC presentation on Xbox Live achievement design highlights Pacifism as a standout achievement. An optional challenge can invite players to discover different ways to use the same controls and arena. [Xbox Live community study, GDC](https://www.gdcvault.com/play/1013706/Achievement-Design-Lessons-From-an)

**Mario Kart 8 (2014).** Nintendo's manual describes saving faster time trial runs as ghost data and sharing ghosts online. A record can serve as both a ranked result and a demonstration that another player can race against. [Nintendo's Wii U manual, Time Trials](https://csassets.nintendo.com/noaext/image/private/t_KA_PDF/manual-WiiU-Mario_Kart_8?_a=DATC1RAAZAA0)

**Trackmania (2020).** Ubisoft describes medals, regional rankings, rival ghosts, and recurring seasonal campaigns. These provide different comparisons for the same skill: improvement against a fixed threshold, location-based standing, and performance alongside another run. [Ubisoft's Trackmania overview](https://www.ubisoft.com/en-us/games/trackmania)

## Techniques and practical checks

### Award achievements from explicit gameplay outcomes

Define a stable achievement ID, an observable completion condition, and the context in which progress is valid. Choose whether progress accumulates across attempts or resets on restart. Persist completion before notifying the player. Retry platform synchronization using the same ID so a repeated delivery has the same result.

**Check** completion just below and at the threshold, repeated completion, reload during the unlock, and offline completion followed by reconnect. Verify that durable progress and the notification follow the intended rules.

### Compare scores within the same rules

Specify units, ordering, tie handling, and the ruleset, course and season that define a board. Keep records from different rulesets distinct when a gameplay update changes what a score means. Choose whether to rank all attempts or retain each player's best eligible result.

**Check** equal scores, duplicate submissions, both faster and slower retries, and two versions of a course. Confirm that rank and pagination are stable and that displayed precision agrees with the tie policy.

### Define when a result becomes official

For shared rankings, accept results from the authoritative match record or a validated run, according to the game model. Distinguish pending, confirmed, and rejected states. Keep personal bests usable offline and apply online updates consistently after reconnect. If a ghost is attached, link it to the exact rules and attempt that produced the record.

**Check** delayed confirmations, repeated delivery, and a failed ghost upload. Verify that the board shows the intended result and that a replay from another ruleset is handled according to compatibility policy.

### Keep comparisons useful as participation changes

Offer a view around the player's placement as well as the leaders, if the board is large. Provide clear scope names and season end times. Define the final standings, reward eligibility if used, and whether archived results remain browseable.

**Check** an empty board, a new player far from the leaders, and a result arriving near rollover. Verify the displayed scope, the season assigned to the result, and any final reward with a repeatable clock-controlled fixture.
