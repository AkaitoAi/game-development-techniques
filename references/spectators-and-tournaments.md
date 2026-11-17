# Spectator clients and tournament operation

## Problem and design reasoning

Spectators and officials need a coherent picture of a match while players need a view governed by competitive rules. A single shared view risks leaking hidden state, forcing observers away from important action, or leaving an official without enough evidence to resolve a dispute. Blizzard documented the value of letting viewers choose a perspective, while Riot documented dedicated tournament infrastructure intended to reduce dependence on external internet connectivity. Tailoring information and operational authority to each role follows from those examples. [Blizzard World Cup Viewer](https://news.blizzard.com/en-us/article/22548747/overwatch-world-cup-viewer-a-new-way-to-experience-esports) · [Riot Worlds engineering](https://www.riotgames.com/en/news/engineering-esports-tech-powers-worlds)

## Shipped examples

- **Overwatch World Cup Viewer (2018).** Blizzard gave viewers first-person and third-person perspectives, free camera, and an overhead map. The documented design gives viewers control over whether to follow an individual or survey the whole fight. At GDC 2017, Blizzard engineer Philip Orwig separately described fitting kill-cam transfer and presentation within respawn deadlines. Together these sources show distinct timeline and viewpoint requirements for participant and audience experiences. [Blizzard viewer announcement](https://news.blizzard.com/en-us/article/22548747/overwatch-world-cup-viewer-a-new-way-to-experience-esports) · [GDC 2017 talk](https://www.gdcvault.com/play/1024053/Replay-Technology-in-Overwatch-Kill)
- **League of Legends at Worlds.** Riot describes portable offline tournament servers designed to run independently of the internet along with on-site esports technology staff. The stated operational aim is resilient competition with low local latency. The applicable lesson is to rehearse failure modes against the actual event topology and separate match-critical connections from broadcast dependencies. [Riot, Engineering Esports](https://www.riotgames.com/en/news/engineering-esports-tech-powers-worlds)

## Techniques and practical checks

### Define spectator roles and information boundaries

Specify the allowed visibility, metadata, communication channels, and delay for player, coach, referee, and audience roles. Enforce each boundary on the authoritative service before handing data to clients. Ensure reconnect and role change maintain the intended delay.

**Check** place a hidden entity and a private team message into a match. Join or reconnect as each role and verify forbidden state stays outside that role's feed. Change a user's authorized role midmatch and verify the correct view and delay.

### Build transitions for observers

Derive HUD state from the current observed subject and match phase. Offer direct selection by player or team, an overview, and predictable fallback on respawn or removal. Give the observer stable team identifiers across camera modes.

**Check** switch rapidly between targets with different health, team, and loadout states. Eliminate or disconnect the observed player. Confirm that HUD, team colors, and camera tracking change together and the fallback remains informative.

### Keep an authoritative result record

Record match identity, participants, build and ruleset versions, server result, and authorized administrative actions. Use a unique result identifier so duplicate submissions resolve to one bracket update. Model remakes and overrides as explicit decisions with an audit trail.

**Check** resubmit an identical result, attempt a conflicting result, and simulate a remake with the same participants. Verify correct bracket advancement, result provenance, and authorized correction.

### Rehearse event failures

Separate the competition server, observer workstation, and broadcast output. Predefine when an official can pause or restart and the conditions for resuming. Verify that loss of an optional view preserves match operation where the rules allow.

**Check** an observer disconnect, broadcast uplink interruption, and game-client restart during a rehearsal. Measure recovery, verify what each role sees, and confirm that final results follow the event rules.
