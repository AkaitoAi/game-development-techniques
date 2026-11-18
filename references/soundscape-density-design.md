# Soundscape density design

## Problem and design reasoning

A scene can stay within its audio voice budget while still sounding crowded. Many audible layers can mask a warning, flatten pacing, or make every location feel equally busy. Frontier's Matthew Florianz describes contextual mix decisions for shipped games including *Planet Zoo* and *Elite Dangerous*, where freely moving cameras and many world objects create large numbers of possible sources. The rationale is to consider the listener's impression and context when choosing what to emphasize. [Frontier, How Distance Has Shaped the Sound of Our Games, GDC 2021](https://www.gdcvault.com/play/1027544/how-distance-has-shaped-the)

For *A Plague Tale: Innocence*, Aurélien Piters documents an organic, textural soundscape and shared variables used by voice, effects, and music. Detection state and guard distance change layers and breathing feedback. This is a shipped example of organizing sound around an encounter's changing state. The density budgets below are design recommendations, with emphasis on listening and gameplay readability rather than physical voice allocation. [Asobo Studio, Audio Design in A Plague Tale, GDC 2020](https://media.gdcvault.com/gdc2020/presentations/A_Plague_Tale_Piters_Aurelien.pdf)

## Techniques and practical checks

### Define an attention hierarchy for each scene

Name the sounds the player needs to identify now, those that explain the location, and those that provide optional texture. Select a few foreground anchors for each state and let secondary layers reduce activity or prominence when those anchors matter. Evaluate spectral and temporal overlap as well as gain.

**Check** replay a busy encounter and ask listeners to identify its warning, objective cue, and location ambience. Repeat after reducing decorative layers. Compare identification accuracy and the perceived identity of the location rather than relying only on meter levels.

### Compose activity over time

Give repeating environmental sources variation in spacing, intensity, and duration. Design quiet intervals and transitions between dense and sparse states. Coordinate emitters that would otherwise all start after the same load or trigger event, and define how activity resumes after a pause.

**Check** listen through a repeatable five-minute route including a return visit, pause, and reload. Mark clusters, repeated coincidences, and intervals with no useful change. Verify important cues stay legible after several minutes of familiarity.

### Connect layers to meaningful world state

Choose a small set of scene variables such as threat, weather, enclosure, or population activity. Map layers to those variables with defined entry and exit behavior. Use smoothing or hysteresis where a noisy boundary would cause the soundscape to alternate rapidly.

**Check** cross a state boundary slowly, reverse direction repeatedly, and trigger a sudden encounter change. Confirm the soundscape communicates the new state within the intended time and avoids rapid layer chatter or lingering danger cues.

### Evaluate the full listening range

Keep the place recognizable in a quiet mix and retain gameplay information during peak activity. Review headphones, speakers, mono playback, and the supported dynamic-range modes. Where a sound carries necessary information, test its available visual or text equivalent too.

**Check** run the same route and dense encounter on each representative output mode at a reasonable low volume. Record missed cues, masking, fatigue, and unexpected silence. Adjust the authored layer balance before increasing the overall level.

See [Audio voice allocation](audio-voice-allocation.md) for resource limits and [Adaptive music](adaptive-music.md) for musical transitions.
