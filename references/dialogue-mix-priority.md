# Dialogue mix priority

## Problem and design reasoning

A line may have a playback voice and still be unintelligible beneath music, effects, or another speaker. This is a perceptual and narrative scheduling problem as well as an allocation problem. Jonathan Lanier's *The Last of Us* presentation describes preserving important dialogue and effects while maintaining an immersive environment, including adapting the mix to game state. It explicitly identifies compromise between cinematic requirements and ordinary audio behavior. [Jonathan Lanier, Aural Immersion: Audio Technology in The Last of Us, GDC 2014](https://www.gdcvault.com/play/1020444)

Jason Gregory's companion presentation describes the shipped game's context-aware dialogue system, which considers individual and collective knowledge, global state, and surroundings while giving sound designers control. That context can help decide whether a line is still worth hearing. The category hierarchy, queue policies, and ducking contract below are recommendations derived from this problem, rather than a claim about Naughty Dog's exact rules. [Jason Gregory, A Context-Aware Character Dialog System, GDC 2014](https://gdcvault.com/play/1020951/A-Context-Aware-Character-Dialog)

## Techniques and practical checks

### Rank lines by what the player needs now

Define categories such as urgent instruction, current conversation, contextual reaction, and ambient chatter. Give each an eligibility window and a policy for interrupting, waiting, or dropping. Queue only lines whose meaning remains valid after the delay. Record why a line was rejected so missing story information can be diagnosed.

**Check** trigger an urgent cue during a conversation, then make the queued contextual line obsolete. Verify the intended audible order, that the story-critical message remains available through the chosen recovery policy, and that the obsolete line stays suppressed.

### Shape masking with bounded mix changes

Create a dialogue-active mix state with measured changes to competing groups, frequency balance, or spatial treatment where appropriate. Set attack, hold, and release behavior to avoid audible pumping. Multiple overlapping lines should share a coherent ownership rule so one line ending does not restore the mix while another still needs protection.

**Check** replay a quiet exchange, a dense encounter, and two overlapping dialogue requests. Compare intelligibility and effect audibility at the same playback level. Verify the mix returns to baseline after all dialogue ends or is cancelled.

### Keep narrative timing and audible timing aligned

Track the actual start and end of the accepted line when driving subtitles or conversation state. Distinguish authored pauses from time spent waiting for priority. Decide how interruption, fast traversal, skipping, and speaker removal affect the remaining exchange.

**Check** interrupt a line near its first and last words, leave its area, skip the exchange, and remove a speaker. Verify subtitle timing, conversation progression, and termination of any mix contribution for that line.

### Validate with player controls and real playback conditions

Keep dialogue, music, effects, and assistive speech controls meaningful under priority rules. *The Last of Us Part I* exposes separate dialogue and other audio volume controls as part of its accessibility features. Test the project's own mix across supported speakers, headphones, and dynamic-range presets. [Naughty Dog, The Last of Us Part I accessibility features](https://blog.playstation.com/2022/08/26/the-last-of-us-part-i-full-list-of-accessibility-features/)

**Check** replay the same scene with low dialogue volume, an accessibility narration cue, a narrow-range preset, and small speakers. Record which lines players understand and whether important non-dialogue cues remain detectable. Check the muted-dialogue experience with subtitles enabled as well.

See [Audio voice allocation](audio-voice-allocation.md) for physical voice limits and [Systemic dialogue](systemic-dialogue.md) for generating eligible lines.
