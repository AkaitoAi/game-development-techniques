# Runtime audio zone programming

## Problem and design reasoning

Audio environments need stable state even when the player crosses overlapping volumes, teleports, or unloads a world cell. Trigger callbacks alone can leave an obsolete ambience or reverb active. *Ghost of Tsushima* required adaptive ambience across a world fourteen times larger than Sucker Punch's earlier worlds, with a small audio team. Apoorva Bansal and Rev. Dr. Bradley D Meyer's GDC 2021 **Big World, Small Team: Designing a Scalable Ambience System for 'Ghost of Tsushima'** describes adapting ambience to region, weather, time, and other parameters. [Developer talk](https://gdcvault.com/play/1027222/Big-World-Small-Team-Designing)

Dragica Kahlina's GDC 2014 **Using AI Techniques to Make Game Soundscapes More Dynamic** describes finite-state-controlled sound sets organized into adjustable sound bubbles. That is a technique presentation, not evidence that *Ghost of Tsushima* uses its architecture. Together the sources motivate separating authored environment rules from sound playback. The lifecycle, reconciliation, and overlap policy below are portable recommendations. [Dragica Kahlina, GDC 2014](https://www.gdcvault.com/play/1020418/Using-AI-Techniques-to-Make)

## Techniques and practical checks

### Derive membership from authoritative spatial state

Maintain a set of candidate zones for each listener or relevant emitter. Use enter/exit notifications to update candidates, with a full membership query after spawning, teleportation, loading, and detected discontinuities. Distinguish listener ambience membership from emitter acoustic membership: a source across a doorway may belong to a different environment.

**Check** spawn inside a zone, teleport between zones without crossing their boundaries, and change floors in stacked rooms. Compare the computed membership with a debug spatial query; confirm that ambience and emitter environment resolve independently as intended.

### Make overlap resolution deterministic

Define whether each property uses a winning zone, weighted blend, additive contribution, or inherited default. Resolve equal priorities with a stable authored rule and report accidental ambiguities. Use hysteresis or a defined transition band where small position changes would otherwise alternate states. Treat ambience selection and acoustic propagation as separate decisions.

**Check** stand on a shared boundary, move slowly across it, and replay the same route with different frame timings. Inspect selected zone IDs, weights, and transitions; require consistent decisions and no rapid oscillation or unexplained loudness buildup.

### Reconcile desired audio against owned instances

Compute desired beds, snapshots, and parameters from resolved zone state, then reconcile them against active instances. Give each instance an owner and generation identifier. Repeated reconciliation should be idempotent; a callback from an older generation must not stop a replacement instance. Blend environmental parameters on an explicit audio transition clock.

**Check** repeatedly request the same state, interrupt a crossfade with a new destination, and delay an old completion callback. Confirm stable instance counts, correct final parameters, and no old callback removing the current ambience.

### Decouple zone data from streamed object lifetime

Keep enough stable zone identity to resolve transitions while associated world objects load or unload. Specify fallback ambience for unavailable data and ownership of outgoing tails. Unregister spatial objects before their references become invalid; cancel pending work or reject it by generation. Do not keep an entire cell resident merely because a fading bed references its scene object.

**Check** unload the source cell during a transition and reload it while asset loading is delayed. Verify fallback behavior, eventual convergence, bounded retained memory, and zero stale handles or orphaned emitters after cleanup.

See [Spatial audio and acoustics](spatial-audio-and-acoustics.md) and [Audio event lifetime management](audio-event-lifetime-management.md).
