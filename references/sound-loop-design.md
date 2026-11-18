# Sound loop design

## Problem and design reasoning

A sustained engine, wind bed, or machine must survive an unknown playback duration. An audible seam breaks continuity; a perfectly seamless but recognizable phrase can still become tiring. Gerard Marino's GDC 2019 **Loop Clinic: Seamless and Fatigue-Free Music and Sound Design Looping Techniques for Interactive Entertainment** treats both continuity and fatigue as authoring problems. [Gerard Marino, GDC 2019](https://gdcvault.com/browse/gdc-19/play/1025942)

Leonard Paul's **The Dynamic Audio of Vessel**, GDC 2012, documents asymmetric loops among the sequencing, layering, granulation, and modulation techniques used for the shipped physics puzzle game. The problem was making rapidly changing liquid interactions feel detailed. In *Ghost of Tsushima*, Apoorva Bansal and Rev. Dr. Bradley D Meyer describe looping ambience alongside one-shot emitters in a scalable world ambience system. These are examples of continuous sound serving simulation and environment; the workflow below is a portable recommendation, not their documented implementation. [Vessel talk](https://gdcvault.com/play/1016014/The-Dynamic-Audio-of) · [Ghost of Tsushima, GDC 2021 slides](https://media.gdcvault.com/GDC%2B2021/33105_AmbienceSystem.pdf)

## Techniques and practical checks

### Design the seam and the playback contract together

Specify sample boundaries, intended playback rate range, channel layout, and whether a file contains an introduction or tail. Match waveform continuity and the perceived room tone across the join. A zero crossing alone does not guarantee matching ambience, phase, or energy. Audition the encoded asset because codecs and playback systems can change boundary behavior.

**Check** render at least twenty consecutive cycles from the shipping playback path. Listen and inspect every join at normal and extreme supported playback rates, including mono playback. Reject clicks, gaps, or unintended gain modulation.

### Separate sustained texture from recognizable events

Keep a stable bed for continuity and schedule distinctive calls, clanks, or gusts as bounded events. Where layers use different periods, choose them deliberately and retain a reproducible scheduling seed. This recommendation follows the documented use of asymmetric loops in *Vessel* without assuming its exact timing rules.

**Check** listen to an uninterrupted session longer than the expected exposure. Mark recurring phrases without looking at the timeline; revise layers whose recurrence becomes distracting, while confirming that important gameplay cues remain identifiable.

### Preserve phase relationships where they matter

For related speed or intensity layers, decide whether synchronization is required. Aligned layers can avoid unstable interference; unrelated textures may benefit from independent phases. Define normalization and blend curves for the whole stack, rather than normalizing each layer and hoping their sum remains controlled.

**Check** sweep every blend slowly and rapidly, including reversals. Compare loudness, spectrum, and stereo compatibility at endpoints and intermediate positions. Confirm that no blend produces an unexplained dip or peak.

### Author starts, stops, and interrupted transitions

Provide attack and release behavior suitable for the object. Choose whether a stop completes a tail, fades immediately, or hands off to a separate shutdown event. Make re-entry during release explicit so rapid toggles do not accumulate copies or restart a conspicuous attack unnecessarily.

**Check** start and stop repeatedly at arbitrary loop positions, then restart during release and destroy the emitter. Capture the output and active voice count; verify expected tails, no clicks, and no surviving voices after the agreed cleanup interval.

See [Sound variation authoring](sound-variation-authoring.md) and [Audio event lifetime management](audio-event-lifetime-management.md).
