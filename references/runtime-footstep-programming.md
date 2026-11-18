# Runtime footstep programming

## Problem and design reasoning

A footstep can play twice when animation and locomotion both emit it, use the wrong surface after a delayed query, or follow an actor that has already been recycled. In the released *Quake III Arena* movement code, ground-surface flags select a footstep event category. Client event handling selects a sound variation and plays it on the entity's body channel. The division makes the movement result available to presentation without asking the audio layer to infer every movement rule. [id Software movement code](https://github.com/id-Software/Quake-III-Arena/blob/master/code/game/bg_pmove.c) · [id Software event playback](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_event.c)

Naughty Dog's *The Last of Us* audio talk describes audio technology supporting immersion, gameplay, and story. Applying an explicit contact-event identity and surface snapshot is a portable implementation recommendation for maintaining that relationship under asynchronous work. It is not a documented claim about Naughty Dog's particular footstep API. [Aural Immersion: Audio Technology in The Last of Us, GDC 2014](https://www.gdcvault.com/play/1020444/Aural-Immersion-Audio-Technology-in)

## Techniques and practical checks

- Give one system ownership of each contact occurrence. For animation-driven steps, identify the actor generation, action instance, foot, and loop occurrence. For movement-driven steps, define a distance or contact transition rule. **Check** blend between walk and run while both clips contain markers. Verify one audible step per intended contact and no accumulated burst after a pause.
- Capture the contact location and relevant surface information at the chosen simulation or animation phase. If a delayed surface query finishes later, validate its owner and revision before using the result. **Check** step across a wood-to-metal seam on a moving platform while delaying query completion. Confirm the result follows the defined sampling policy and cannot change a replacement actor's sound.
- Separate footstep variation from gameplay rules. Choose sample families from material, movement type, and footwear as needed, then apply bounded variation and per-actor limits. Use confirmed gameplay contact for effects that alter game state. **Check** replay sprinting, crouching, landing, and shallow-water movement. Inspect event type, sample family, gain, and rejected-event counts under a crowded scene.
- Define what happens on teleport, pooling, loss of contact, and scene exit. Retire queued contacts and reset stride history when the previous movement segment is no longer meaningful. **Check** teleport during a stride and return an actor to a pool with a sound queued. Verify no stale step plays at the new location and active event records return to baseline.

See [Animation event routing](animation-event-routing.md) and [Audio event lifetime management](audio-event-lifetime-management.md).
