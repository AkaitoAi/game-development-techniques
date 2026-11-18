# Audio transition programming

## Problem and design reasoning

An ambience or music change can be correct in isolation yet click, dip, or restart when another request arrives mid-fade. This guide covers how a transition progresses and is interrupted after the desired audio state has been selected. Use runtime audio zones for spatial membership and event lifetime management for individual voice ownership.

For **Ghost of Tsushima**, a small team had to populate a world much larger than the studio's previous work. Apoorva Bansal and Rev. Dr. Bradley D Meyer's GDC 2021 talk, **Big World, Small Team: Designing a Scalable Ambience System for 'Ghost of Tsushima'**, describes adaptive ambience responding to time, weather, region, and other parameters. Multiple changing inputs motivate explicit transition policies; the request protocol below is a portable recommendation, not a claim about their internal implementation. [Sucker Punch, GDC session](https://gdcvault.com/play/1027222/Big-World-Small-Team-Designing)

Audiokinetic's authoring documentation describes source/destination transition rules, optional bridge segments, and a fallback rule for unspecified pairs. This provides a concrete model for making transitions authored behavior rather than unrelated stop/start calls. [Audiokinetic, The Wwise Project](https://www.audiokinetic.com/download/documents/WwiseProjectAdventure_en.pdf)

## Techniques and practical checks

### Represent the destination separately from the current blend

Maintain requested state, audible state, and transition progress separately. Give requests an owner and generation. Coalesce repeated requests for the same destination; document whether a higher-priority request replaces, queues behind, or temporarily overrides the current one. Avoid letting every weather, combat, and menu subsystem start its own competing crossfade.

**Check** submit identical requests every frame, then alternate two destinations rapidly. Count active layers and pending operations. Repetition must not restart the same transition or accumulate silent voices.

### Retarget from what is currently audible

When replacing a fade, start from current gains and parameter values rather than the previous state's endpoints. Use a curve suitable for the sources: correlated loops may become louder with an equal-power crossfade, while unrelated layers may dip under a linear one. Treat filter and send interpolation as part of the transition.

**Check** interrupt at several points with a third destination and reverse immediately afterward. Capture the output and inspect both perceived loudness and parameter continuity. Listen for clicks, doubled transients, and sudden filter changes.

### Schedule musical changes against an audio timeline

For music, distinguish request time from the permitted exit boundary. Specify beat/bar markers, bridge behavior, loading readiness, and a maximum acceptable wait. If a replacement arrives before the boundary, explicitly replace the scheduled destination or finish the committed bridge. Do not derive musical position from rendered frame count.

**Check** request changes just before and after boundaries under low frame rate and delayed asset readiness. Confirm the selected policy, musical alignment, and bounded response time rather than merely checking that the new track eventually plays.

### Retire outgoing layers after their intended tail

Separate stopping new excitation from destroying a processing chain. Preserve a designed reverb or release tail, with a duration or silence threshold and a hard budget. Use generation checks so completion of an old fade cannot stop a newly reused layer. Define pause, teleport, and scene unload policies explicitly.

**Check** unload during a bridge, pause halfway through a crossfade, and reuse an audio layer before an old completion fires. Verify tails finish or follow the documented cutoff policy, stale completions have no effect, and active layer counts return to baseline.
