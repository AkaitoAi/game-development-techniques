# Interactive Foley systems

## Problem and design reasoning

Foley connects movement and contact to a believable material response. A repeated sample can reveal the implementation, while layering every physical contact can mask useful information. In *Unpacking*, Jeff and Angela van Dyck produced a large library of object sounds. Their GDC account documents the production complexity behind interactions that appear simple to the player. [Jeff van Dyck, Unpacking: The Fun Behind the Foley, GDC 2022](https://www.gdcvault.com/play/1027728)

Steve Whetman and Byron Bullock describe using Foley to support tension in *Alien: Isolation* and weight in *Gears of War: Ultimate Edition*. These are distinct presentation goals. A portable design inference is to choose source material and playback rules from what the interaction should communicate, then budget variation and layering around the actual frequency of play. The event and material contracts below are recommendations. [Whetman and Bullock, Foley in Alien: Isolation and Gears of War: Ultimate Edition, GDC 2016](https://www.gdcvault.com/play/1023223/Foley-in-Alien-Isolation-Gears)

## Techniques and practical checks

### Classify the interaction before selecting a sound

Describe the action, contacting materials, intensity, and perspective as separate inputs. Author fallback categories for unknown material combinations. For movement, use a stable foot or contact identity so the system can distinguish a fresh step from a sustained contact.

**Check** replay the same movement over two known surfaces and an unclassified surface. Log the selected category and variation. Verify each valid contact has a useful response and missing data uses the intended fallback.

### Align sound to the authoritative contact cue

Choose animation markers, physics contacts, or a gameplay interaction event according to the action's timing contract. If several channels describe the same occurrence, correlate them before playback. Use continuous events for rolling or scraping and an explicit stop when the relationship ends.

**Check** vary animation speed, interrupt a step, drop a prop, then roll it to rest. Verify no duplicate onset, no landing sound before contact, and no persistent scrape after pickup or destruction.

### Design variation and layers around repetition

Use a controlled variation pool and avoid immediate repeats where audible. Reserve layers for information the player can perceive, such as equipment movement or a different surface response. Set sensible per-action and per-emitter limits before routing the sounds into the broader voice allocator.

**Check** listen to a long repeated walk and then a crowded encounter. Compare audible repetition, simultaneous voice use, and the clarity of important cues. Include a single-variation fallback to verify that the selector still terminates.

### Review the result in its gameplay mix

Audition Foley against dialogue, music, and ambience at player listening distance. Use the game's perspective policy consistently when camera and character positions differ. Preserve meaningful contact information with visual or haptic feedback where the game requires an alternative channel.

**Check** perform the same interaction with headphones, speakers, a mono output, and muted audio. Verify the sound's apparent position and material, then check whether required gameplay outcomes remain recognizable through the available alternatives.

See [Audio event lifetime management](audio-event-lifetime-management.md) for event ownership and [Audio voice allocation](audio-voice-allocation.md) for runtime budgets.
