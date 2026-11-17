# Voice production and localization

## Problem and design reasoning

When scripts, performances, and localized recordings change independently, the build can play a stale line, select the wrong actor or language, or cut off a dub timed to another version. Treat a voice line as a production item with context, stable identity, revision, and acceptance criteria. That structure makes changes visible before asking actors to repeat work.

**Documented.** CD Projekt Red's localization team describes close cooperation between localization and engineering as crucial to shipping *Cyberpunk 2077* voice content across languages. Ubisoft's VoRACE talk describes reducing the manual cost of preparing recorded dialogue while keeping the tools creator-assisted. **Inference.** It is worth giving specialists a shared line manifest and using automated checks to surface exceptions for listening review. [Tommi Nykopp and Mikolaj Szwed, GDC 2023](https://www.gdcvault.com/play/1029219/Localization-of-Cyberpunk-2077-Technology) · [Jean-Philippe Letendre, GDC 2023](https://www.gdcvault.com/play/1029419/VoRACE-Tools-for-Automating-Dialogue)

## Shipped examples

- **Cyberpunk 2077 (2020)** required recording coordination for more than 80,000 dialogue lines in 11 voice-over languages. Its localization team presented the recording preparation, data pipelines, and collaboration needed to support that scope. [Localization of Cyberpunk 2077, GDC 2023](https://www.gdcvault.com/play/1029219/Localization-of-Cyberpunk-2077-Technology)
- **Ghost of Tsushima Director's Cut (2021)** added Japanese lip sync for PS5 cinematics. The announcement attributes the change to real-time cinematic rendering on that platform, showing that the presentation path can determine what a dub can change. [Sucker Punch announcement, PlayStation Blog](https://blog.playstation.com/?p=350643)
- **Uncharted 2: Among Thieves (2009)** added live audio recording on the motion capture stage as the team developed its performance capture pipeline. This preserved an on-set recording of the interaction among actors for downstream cinematic work. [Naughty Dog postmortem](https://media.gdcvault.com/GD_Mag_Archives/GDM_March_2010.pdf) · [Amy Hennig and Josh Scherr, GDC 2010](https://www.gdcvault.com/play/1012807/Behind-the-Scenes-Uncharted-2)

## Techniques and practical checks

### Record with gameplay context

Prepare a session packet with line ID and revision, speaker, listener, preceding line or trigger, pronunciation notes, and delivery and timing constraints. Include picture or a reference take for timed scenes. Track approved takes and pickup notes during the session. [Adam Ritchie, Learnings from Working with Voice Actors Remotely, GDC](https://gdcvault.com/play/1034922/Learnings-from-Working-with-Voice)

**Check** a first-time read for a conversational line, an interrupted line, and an offscreen cue. Have the director identify what the player sees, what the actor reacts to, and how much time the performance has. Repair context gaps in the packet.

### Version scripts and recorded assets together

Keep a stable ID for the triggering line and track revisions separately. Store the locale, speaker, selected take, status, and delivery file in the manifest. Mark language versions for review when source meaning, casting, or timing changes. Give editorial a report of missing, stale, and unexpected files.

**Check** a single-line pickup in one language and a meaning change in the source line. Verify the revision report flags the expected recordings, a build picks the approved pickup, and unrelated banks avoid a needless rebuild.

### Budget timing and decide how each presentation adapts

Distinguish flexible delivery, fixed cinematic windows, and overlaps where a trigger decides when speech ends. Plan editorial, writing, subtitle timing, and lip sync together. Where a localized line has additional duration, decide whether to adapt the performance, adjust the camera and timing, or provide a longer window.

**Check** short and long localized takes against the same camera cuts and gameplay triggers. Listen for clipping and crowded handoffs, check subtitles stay synchronized with the selected voice, and inspect visible mouth motion for close-ups.

### Combine batch validation with in-game listening

Automate checks for missing files, silence, unexpected sample format, and deviations from the project's loudness or peak range. Keep ambiguous edits and pronunciation decisions for review. Spot-check packaged content under gameplay conditions and define what happens when a language pack is unavailable.

**Check** in at least two available locales with cinematics, reactive barks, cancellation, and audio ducking active. Test a deliberately unavailable voice asset and confirm the intended fallback or error is legible without mislabeling the spoken language.
