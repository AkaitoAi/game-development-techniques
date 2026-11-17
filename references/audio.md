# Game audio

## Problem and design reasoning

**Problem.** Players cannot locate sound sources, or individually authored effects fail to cover the combinations a systemic game can produce.

**Design reasoning.** Nintendo documents component sounds for a wagon in Tears of the Kingdom. A design inference is to compose audio from the interacting parts when combinations are unbounded, while reserving specific authored sounds for outcomes that need immediate recognition.

## Shipped examples

**Doom (1993).** The released Linux port adjusts a world sound's gain by distance and its stereo position by bearing relative to the listener. The runtime updates these parameters for active sounds and releases voices that become inaudible. Finite voice allocation and inexpensive spatial cues made direction and distance legible on stereo playback. [id Software, released Doom sound code](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/s_sound.c)

**The Legend of Zelda: Tears of the Kingdom (2023).** Nintendo's audio team described distance- and environment-dependent filtering and demonstrated how a wagon's sound emerges from its rolling wheels, moving frame, and chain. Reusable physical sound behavior lets arbitrary object combinations produce recognizable results. Gameplay cues such as item pickups can still use deliberately authored sound effects. [Nintendo developers, GDC 2024 presentation](https://gdcvault.com/play/1034667/Tunes-of-the-Kingdom-Evolving)

Both implementations map world state to audible properties. The modern example extends that mapping to more input parameters and object combinations. Distance and bearing alone can remain sufficient where players primarily need to locate the source. Add environmental processing or individual component sounds when they convey useful information and fit the runtime budget.

## Techniques and practical checks

### Define the listener and tune attenuation

Choose a listener position and orientation appropriate to the viewpoint. For an offset third-person camera, decide which position controls loudness and which orientation controls panning. Set a close range, a falloff curve, and a far cutoff for each important class of sound. Smooth optional occlusion filtering when a wall or door obstructs the path. Keep interface confirmations on a separate nonspatial route.

**Check** with a fixed source at known left, right, near, and far locations. Rotate the camera, approach through a shut door, and repeat with headphones, stereo speakers, and a mono downmix. Verify directions, distances, and the audibility of essential cues.

### Turn contacts into sound parameters

Send confirmed contact onset events with a position, material pair, and strength estimate such as normal impulse. Author material-pair defaults and map strength to gain or sample variation. Use separate loops for sustained rolling or scraping, controlled by relative motion. Merge repeated contact events within a short interval and fade loops when contact ends.

**Check** by dropping the same prop on different surfaces, rolling it to rest, piling up several props, and teleporting or destroying an active emitter. Record which events started or stopped voices. Listen for chatter, repeated attacks, abrupt cuts, and stuck loops.

### Schedule music on musical boundaries

Model the score in sections or layers, with a musical clock shared by synchronized parts. Choose the allowed transition boundary per change, such as the next beat, bar, or authored marker. Resolve conflicting requests before that boundary. Use hysteresis for conditions that change rapidly, and specify whether a pause preserves or restarts the musical phase. [LucasArts, Land and McConnell's interactive music patent](https://patents.google.com/patent/US5315057A/en)

**Check** by issuing requests one frame before and after a transition boundary, switching states repeatedly, and pausing or loading during a queued transition. Capture the output and check the transition time, rhythmic continuity, and alignment across layers.

### Prioritize gameplay cues under load

Separate dialogue, music, ambience, and effects into mix groups. Measure peak voice use and reserve capacity or assign priority for cues that affect decisions. Apply per-emitter or per-event limits to repetitive sounds. When dropping a voice, prefer one with low importance and low audibility. For cues that carry essential information, provide a synchronized visual or text alternative where appropriate.

**Check** by replaying the busiest encounter with voice limits enabled. Log active voices, rejected events, and audio thread time. Verify that important cues survive, clipping and underruns stay within the project's acceptance limits, and the same information remains available with audio muted.
