# Sound cue readability

## Problem and design reasoning

A cue can have a playback voice and still fail to communicate an action. Its vocabulary, timing and resemblance to other sounds determine whether players can identify it. *The Last of Us Part I* documents traversal and combat cue sets and an audio glossary. Lanier's GDC talk on the original game explains the need to communicate tactical information while preserving important speech and effects. The recognition tests below are recommendations derived from those goals. [Naughty Dog, The Last of Us Part I accessibility features](https://blog.playstation.com/2022/08/26/the-last-of-us-part-i-full-list-of-accessibility-features/) · [Jonathan Lanier, Aural Immersion, GDC 2014](https://www.gdcvault.com/play/1020444/Aural-Immersion-Audio-Technology-in)

## Techniques and practical checks

### Define the cue vocabulary

Map each cue to an action, state or result. Keep nearby meanings distinguishable through rhythm, timbre or pitch contour and document their meaning in an accessible reference.

**Check** Play randomized cues to first-time players after a short introduction. Record mistaken pairings and whether they can choose the intended action.

### Place cues at decision time

Identify how much warning a response needs and emit the cue from the gameplay event that owns it. Distinguish warning, successful action and failure.

**Check** Capture a known action at normal and low frame rates. Compare cue onset with the decision window and verify players receive useful warning.

### Test under masking

Author variations that preserve the identifying feature. Use the shipping mix, representative distractions and supported playback modes.

**Check** Test the same randomized cues during dense gameplay on speakers, headphones and mono. Record recognition and reaction errors rather than volume alone.

### Make feedback adjustable

Expose suitable cue controls and pair essential information with visual or haptic feedback where supported. Keep alternatives synchronized with the event.

**Check** Reduce or mute audio and exercise the same scenario with available alternatives. Confirm players can identify the same actionable state and that disabled cues leave no misleading leftovers.
