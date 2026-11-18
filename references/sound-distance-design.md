# Sound distance design

## Problem and design reasoning

Players infer distance from more than loudness. A quiet nearby threat can be mistaken for a distant one, while a loud sound through a wall can imply a route that does not exist. IO Interactive's *Hitman* talk identifies the failure of ordinary distance attenuation when a nearby source occupies another space and describes a workflow supporting open spaces, enclosed spaces, and dynamic geometry. [Stepan Boev, Sound Propagation in Hitman, GDC Europe 2015](https://gdcvault.com/play/1022824/Sound-Propagation-in)

In *Returnal*, Housemarque describes using positional audio for battlefield awareness, prioritizing unseen enemies, and changing weapon reflections with surrounding geometry. Capcom describes sound as a way to establish an enemy before it becomes visible in *Resident Evil Village*. These are documented communication goals. The near, middle, and far profiles below are authoring recommendations derived from those goals. This guide addresses the perceived meaning of distance. See [Spatial audio and acoustics](spatial-audio-and-acoustics.md) for propagation architecture. [Housemarque and Capcom developer commentary](https://blog.playstation.com/2021/05/10/ps5s-3d-audio-returnal-resident-evil-village-creators-detail-tempest-implementation/)

## Techniques and practical checks

### Give each sound class an audible-distance purpose

Define the range at which a sound should be noticed, identified, and acted upon. Author a distance profile using gain, filtering, transient detail, and direct-to-reverberant balance as appropriate. Keep critical warning reach consistent with the time the player needs to respond.

**Check** place one representative source at marked near, middle, and far positions. Ask players to estimate proximity and identify when they would act. Compare their responses with the intended action range on headphones and speakers.

### Keep distance transitions continuous

Blend layers or parameter changes across authored ranges. Preserve a source's identity while detail changes, and define behavior for very close positions and sources passing the listener. Account for output modes whose spatial detail differs.

**Check** move the source slowly across every profile boundary, then pass it at the fastest supported relative speed. Listen for level jumps, timbre discontinuities, or a source that abruptly changes apparent identity.

### Reconcile occlusion with authored distance

Use the propagation or occlusion result to shape what arrives through an opening or obstacle. Choose how much location information should survive obstruction for the sound class. Keep an important cue recognizable when its direct path is reduced.

**Check** compare the same source in clear view, behind a wall, through a doorway, and around a corner at comparable geometric distances. Ask players to locate the likely route. Verify opening a door changes the impression smoothly.

### Validate distance in the full mix

Evaluate the profile against music, dialogue, ambience, and simultaneous effects. Preserve critical near-threat information under load and provide an appropriate alternative cue where essential information otherwise depends on hearing.

**Check** repeat the distance fixture during the busiest representative encounter, at the lowest supported mix preset, and with accessibility settings enabled. Record missed warnings and incorrect distance judgments alongside voice count and mix changes.
