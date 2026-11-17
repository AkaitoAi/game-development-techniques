# Simulation command buffers

## Problem and design reasoning

Removing entities or changing their component membership while systems iterate can invalidate views or make outcomes depend on worker timing. Deferred commands allow systems to describe structural changes and commit them at a named barrier. Blizzard's GDC account of shipped Overwatch identifies deterministic simulation as important to multiplayer responsiveness. The buffer design here is a transferable technique for controlling mutation order. Unity's Entities documentation demonstrates ordered command playback using sort keys independent of worker scheduling. [Timothy Ford, Overwatch Gameplay Architecture and Netcode, GDC 2017](https://gdcvault.com/play/1024001/contactUs) · [Unity Entities command buffer playback](https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/systems-entity-command-buffer-playback.html)

## Techniques and verification

- Pick a barrier after all producers finish and before consumers require the new structure. Document what remains visible between recording and playback. **Check** a request recorded during iteration. Assert visibility both before and after the barrier and check that a later system sees the intended result.
- Make ordering explicit for noncommuting operations. Derive sort order from simulation data, with a stable tie breaker for equal priorities. **Check** replay identical input with varied worker counts and injected scheduling delays. Compare ordered command logs and state hashes.
- Resolve conflicts in a single place. Define what happens when despawn competes with an update, when a target was recycled, and when two producers request the same spawn. **Check** submit competing operations and verify a stable outcome with useful rejection diagnostics.
- Give records a validated lifetime and treat the commit boundary as part of any snapshot or rollback protocol. Bound deferred work so large spawn bursts do not create unexplained stalls. **Check** restore immediately before and after a barrier and repeat with a dense spawn burst. Verify no command is silently lost or applied twice and measure playback cost.
