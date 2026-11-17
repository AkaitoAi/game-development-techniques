# Game runtime feature flags

## Problem and design reasoning

Releasing a binary and enabling an optional feature have different failure modes. A client may contain the feature while the backend is unavailable, or an operator may need to stop new uses while preserving active sessions. Riot's *League of Legends* replay launch separated these steps. The team deployed server components in advance, enabled traffic gradually by region, and shipped client integration before turning it on through dynamic configuration. Riot's stated goal was to manage delivery risk for a widely requested feature. [Jennie Lees and Sela Davis, Stop Killing Our Servers: Live Edition, GDC 2017](https://media.gdcvault.com/gdc2017/Presentations/Lees_Stop%20Killing%20Our.pdf)

Bungie documented temporarily disabling the Loadouts function in shipped *Destiny 2* after an issue. The announcement establishes the player-facing outcome, while the underlying configuration mechanism is unspecified. A transferable inference is to define what disabling a feature means for entry points, persisted data, and already accepted work. [Bungie, This Week in Destiny, August 24, 2023](https://www.bungie.net/7/en/News/Article/this-week-in-destiny-8-24-23)

## Techniques and practical checks

### Define an enablement contract

Give each flag a typed key, owner, fallback, supported build range, application boundary, and retirement condition. If enablement requires a service capability, content pack, or second flag, express the dependency in the resolver and record the resolved decision.

**Check** start with each dependency missing in turn and with a malformed value. Verify deterministic fallback and a diagnostic explaining what prevented enablement.

### Make one decision per relevant scope

Resolve shared gameplay rules on the authoritative side and include the policy revision in match or session state. Choose whether client-only presentation flags can change immediately. For a shared rule that changes during play, apply it at a defined safe boundary and communicate the change to affected clients.

**Check** a fresh join, reconnect, and late join during a configuration change. Confirm all participants evaluate the same gameplay rules and that client controls agree with server decisions.

### Define what disable does to active work

Map the disabled state across every initiation path. Decide whether accepted work finishes, pauses, or is cancelled with recovery. Keep owned progression and inventory governed by their normal persistence rules. A player-facing explanation helps distinguish temporary unavailability from lost access.

**Check** disable the flag while a request is queued, processing, and completed. Verify that retries do not duplicate grants and that re-enabling restores access to eligible prior state.

### Validate configuration before applying it

Resolve a complete configuration revision before swapping it into use. Keep a tested startup default and define when a cached revision expires. Capture revision metadata in debugging traces and use the same replay or test fixture to compare enabled and disabled paths.

**Check** restart offline, return an unsupported revision, and roll back after a staged change. Confirm the game chooses the intended fallback and records which revision actually governed the session.
