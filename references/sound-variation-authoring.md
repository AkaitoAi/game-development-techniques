# Sound variation authoring

## Problem and design reasoning

Frequent playback exposes repeated recordings, but unconstrained variation can make the same action change apparent size, intensity, or identity. Jonatan Crafoord's *Angry Birds 2* slides document using pitch variation, extra recordings for recognizable or frequently repeated sounds, and nonrepeating selection among small variant sets. The presentation explains fitting varied audio within a small mobile footprint. [Jonatan Crafoord and Elvira Björkman, Angry Birds 2: Next Gen Audio in Only 10 Megabytes, GDC 2016](https://www.gdcvault.com/play/1023217/Angry-Birds-2-Next-Gen) · [Developer slides, sound summary](https://media.gdcvault.com/gdc2016/Presentations/crafoord_jonatan_angry%20birds%202.pdf)

Jeff van Dyck's *Unpacking* talk describes the unexpectedly large Foley production effort behind its object interactions. That supplies a different shipped-game constraint: an extensive sound library still needs to be organized around what each object and action communicates. The classification, acceptance ranges, and test procedures below are recommendations for authoring variation; the sources do not establish that either game used this exact workflow. [Jeff van Dyck, Unpacking: The Fun Behind the Foley, GDC 2022](https://www.gdcvault.com/play/1027728)

## Techniques and practical checks

### Define what every variant must preserve

Group recordings by action, material, intensity, and perspective before random selection. Name the qualities that can vary and those that must remain recognizable, such as a UI success cue or the weight of a heavy impact. Remove outliers that imply the wrong state before trying to hide them with low probability.

**Check** audition every eligible variant in the same representative scene at matched playback settings. Ask listeners to identify the action and intensity without seeing the event. Review variants whose meaning differs from the rest of the group.

### Author repeat rules for the actual event rate

Choose weighted random, shuffled bags, or sequential selection according to the desired behavior. Define whether recent-history exclusion belongs to an emitter, an event family, or the whole scene. Handle one- and two-recording sets explicitly, since a requested exclusion window can exceed the available set.

**Check** trigger one emitter slowly, then several emitters rapidly. Log selected recording IDs and compare immediate-repeat rate and long-run selection frequency with the authored rules. Repeat after pooled emitters are returned and reused.

### Bound parameter variation by perception

Specify allowed pitch, gain, timing, and spatial variation per event family. Keep important attack transients and synchronization inside the gameplay cue's timing budget. Record variation parameters separately from source files so tuning can be reviewed and reproduced.

**Check** play the extrema and common combinations, including the quietest low-pitch and loudest high-pitch results. Verify that every result still communicates the intended action and fits the mix, then inspect repeated events against animation timing.

### Spend assets where repetition is noticeable

Measure how often sounds play and where listeners notice repetition. Add recordings or layers for exposed families and use smaller sets where controlled processing is effective. Track compressed size, decoded memory, and simultaneous voice cost independently.

**Check** capture a sustained gameplay route with a fixed variation seed, then an independent seed. Listen for recognizable loops, compare family-level repetition reports, and measure memory and voice peaks. Re-run after compression to check transients and noise tails.

See [Interactive Foley systems](interactive-foley-systems.md) for event classification and [Audio voice allocation](audio-voice-allocation.md) for deciding which simultaneous sounds are heard.
