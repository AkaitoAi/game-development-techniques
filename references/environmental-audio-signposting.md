# Environmental audio signposting

## Problem and design reasoning

Players can hear a believable world yet miss the route or tactically important location. Jonathan Lanier's GDC 2014 **Aural Immersion: Audio Technology in The Last of Us** explicitly discusses using the environment to lead players toward or away from tactical areas while maintaining dialogue clarity. The design reasoning is to coordinate spatial meaning, mix, and level intent rather than treating environmental sound as decoration. [Lanier, GDC 2014](https://www.gdcvault.com/play/1020444/Aural-Immersion-Audio-Technology-in)

*The Last of Us Part I* also documents Enhanced Listen Mode, which emits cues at scanned targets and varies pitch with relative height. This shipped assistance example makes location meaning explicit. The authored landmark workflow below is a broader recommendation, and environmental atmosphere should not be assumed sufficient for accessible navigation. [Naughty Dog, accessibility features](https://blog.playstation.com/2022/08/26/the-last-of-us-part-i-full-list-of-accessibility-features/)

## Techniques and practical checks

### Give each landmark a navigational purpose

Mark decision points and intended destinations on the level plan. Assign recognizable environmental sources to selected landmarks, such as machinery at an exit or activity beyond a courtyard. Document whether the cue invites approach, warns of danger, or helps the player reorient after returning.

**Check** ask first-time players to explore without route coaching. Record their first turn, wrong turns, and stated interpretation of the source, then compare those observations with the landmark's intended role.

### Preserve a believable directional path

Place sources where their apparent direction supports reachable space. Doorways and connecting passages may need authored presentation that remains consistent with the world's acoustic rules. Review locked doors, vertical overlap, and one-way paths so an attractive source does not repeatedly suggest an impossible shortcut.

**Check** approach from every permitted entrance and floor, including the wrong side of a locked connection. Confirm the audible direction supports a valid decision or communicates a deliberate mystery with a later payoff.

### Schedule discoverable moments

Use repetition intervals, quiet windows, and contextual mix priorities to give the player another opportunity to notice a landmark. Let important speech finish when simultaneous playback would hide the clue. Retire or change cues when the destination's purpose changes.

**Check** arrive during combat, dialogue, and a missed cue cycle. Measure whether the next useful opportunity occurs before players abandon the area and verify completed objectives stop attracting unnecessary revisits.

### Validate navigation across perception modes

Test the shipping mix in stereo, mono, headphones, and supported speaker configurations. Pair essential route information with appropriate visual, haptic, or explicit navigation assistance. Evaluate route success separately from simple sound recognition.

**Check** repeat the same route with audio reduced, directional information unavailable, and supported assistance enabled. Record completion, hesitation, and wrong-floor errors, and ensure an essential objective remains findable through the provided alternatives.

See [Sound cue readability](sound-cue-readability.md) for recognition tests and [Spatial audio and acoustics](spatial-audio-and-acoustics.md) for acoustic implementation.
