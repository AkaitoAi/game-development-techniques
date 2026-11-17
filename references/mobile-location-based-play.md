# Mobile location-based play

## Problem and design reasoning

Location signals fluctuate near buildings and indoors. If one sample opens or closes an encounter, players can lose access while standing still. Niantic describes designing *Ingress* around real-world points of interest and using exploration as a goal in *Pokémon GO*. A transferable inference is to treat proximity as uncertain while preserving the incentive to explore. [Niantic developer interview](https://developer.samsung.com/sdp/blog/en/2020/06/22/best-of-galaxy-store-awards-2019-winner-get-outside-with-niantic-labs-greg-borrud) · [Niantic at GDC Summer, AR Game Design 101](https://gdcvault.com/play/1026885/AR-Game-Design)

## Techniques and practical checks

### Stabilize proximity

Use separate enter and exit radii and define how age and reported accuracy affect sample eligibility.

**Check** Replay noisy samples on a boundary and a stale sample after resume. Confirm a stable encounter state and a clear near-boundary message.

### Limit location access

Request the minimum access when the feature is invoked and provide a way back if access is denied.

**Check** Try denied, approximate, and revoked permission. Confirm the game explains which action is unavailable and unrelated play still works.

### Review real-world affordances

Give entry points accessible public approaches and provide a way to report unsafe or inaccessible locations.

**Check** Review mapped access with representative local routes. Test obstructions and ensure the UI does not instruct players to enter unsafe areas.

### Handle uneven map coverage

Measure playable density across region types and give sparse areas suitable alternatives.

**Check** Compare entry times in dense, suburban, and rural test areas and verify the fallback is discoverable.
