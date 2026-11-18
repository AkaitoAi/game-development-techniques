# Audio emitter authoring

## Problem and design reasoning

A waterfall that sounds like a tiny point, a vehicle engine attached to the wrong pivot, or an inaudible warning undermines the relationship between visible action and sound. Author the physical source representation separately from environmental zone membership and playback lifetime.

For *Ghost of Tsushima*, Apoorva Bansal and Bradley D Meyer describe building adaptive ambience with a small team for a world fourteen times larger than the studio's previous work. Their GDC 2021 talk establishes the production need for scalable population rather than hand tuning every location. It does not prescribe the emitter schema below. [Big World, Small Team](https://gdcvault.com/play/1027222/Big-World-Small-Team-Designing)

An older concrete example is *Quake III Arena*. Its released client code submits entity looping sounds using the entity's interpolated origin. Microsoft separately demonstrates updating emitter and listener transforms for positional audio. These sources motivate an explicit transform contract. The following authoring workflow is an engine-agnostic recommendation. [id Software, entity effects](https://github.com/id-Software/Quake-III-Arena/blob/master/code/cgame/cg_ents.c) · [Microsoft, Using positional audio](https://github.com/Microsoft/DirectXTK/wiki/Using-positional-audio)

## Techniques and practical checks

### Declare the audible source

Store event identity, source shape, attachment, orientation, attenuation preset, and authoring owner. Use a point for a localized machine and evaluate a supported distributed or area representation for an extended river. Multiple independent loops can accumulate energy, so define whether positions represent one sound or several.

**Check** walk around and through a long source with visualization enabled. Listen for sudden panning, gaps, and volume increases where representations overlap.

### Attach to the sound-making part

Choose a stable socket or local offset rather than assuming the object's pivot is appropriate. Specify whether a one-shot follows its owner or remains at its trigger location. Supply velocity consistently if Doppler is enabled.

**Check** rotate, animate, reparent, and teleport a vehicle or creature. Compare source markers with the audible part and verify a stationary impact stays at its impact point.

### Review attenuation along player paths

Define useful listening distance from gameplay purpose and mix conditions. Separate decorative coverage from cues needed to make decisions. Evaluate listener placement for distant cameras and multiple local players.

**Check** replay approach, retreat, obstruction, and camera-offset paths under the busiest mix. Record the distance at which the cue becomes identifiable.

### Validate placement as content

Report missing events, invalid attachments, duplicated emitter IDs, excessive density, and sources inside unintended geometry. Save audition routes with level revisions.

**Check** introduce one invalid attachment and one duplicate emitter. Confirm validation locates both, then measure active voices on the densest audition route.

See [Runtime audio zone programming](runtime-audio-zone-programming.md) for environmental membership and [Audio event lifetime management](audio-event-lifetime-management.md) for playback ownership.
