# Game localization and internationalization

## Problem and design reasoning

**Problem.** Translated dialogue overflows its timing or layout, or a word puzzle loses the logic that made it playable in the original language.

**Design reasoning.** Alexander O. Smith's account of adapting Al Bhed shows tailoring the technique to the target language while preserving the player's chance to decipher it. Identify which outcomes a translation must preserve before reviewing the words in isolation.

## Shipped examples

**Final Fantasy X (2001).** Localization specialist Alexander O. Smith recalls rewriting dialogue to fit scene timing during English voice production. The English version also adapted the Al Bhed language puzzle from syllable substitution to letter substitution, preserving the player's ability to decipher the text gradually. Localize the gameplay function of the language alongside its meaning. [Alexander O. Smith interview](https://www.rpgsite.net/interview/3030-alexander-o-smith-interview)

**Hades II (2025).** Supergiant planned to deliver localization work in batches during Early Access and asked its translators to review relevant feedback from the team and community as development continued. This is an example of keeping localization integrated with repeated public content releases. [Supergiant's localization brief](https://www.supergiantgames.com/blog/job-opportunity-translators-for-hades-ii/) · [Supergiant's 1.0 announcement](https://www.supergiantgames.com/blog/hades2-now-available/)

## Techniques and practical checks

### Give each message context and a stable identity

Store player-facing text behind stable IDs. Attach the speaker, recipient, scene, tone, and placeholder meanings to the source entry. Offer a screenshot or playable fixture for ambiguous UI strings. Let translations reorder named arguments and use plural categories appropriate to the locale.

**Check** reused short strings in different contexts, counts of zero and one, reordered arguments, and missing entries. Verify exported text preserves its context and that import rejects damaged placeholders without discarding unrelated translations.

### Render languages before the final text arrives

Prepare a pseudo-locale with expanded strings, visible markers, and glyphs outside the default character set. Render and wrap by grapheme boundaries and test your actual font fallback, shaping, and bidirectional text support with representative scripts. Allocate flexible layout space for subtitles and controls.

**Check** dense menus, controller prompts, subtitles, and narrow screens. Include combining marks, mixed-direction numbers and names, and a language whose usual word breaks differ from those of the source. Confirm that clipping and missing glyphs are captured by automated screenshots and native-language review.

### Localize timing and language-dependent rules together

Supply dubbing scripts with reference video, cue IDs, context, pronunciations, expected duration, and any hard timing constraints. Keep puzzle clues, answers, and interaction feedback in the same translation bundle. Use locale-appropriate equality or normalization rules where typed answers matter.

**Check** actual recorded takes against cue starts, animation, subtitle timing, and the next line. Have reviewers play each localized puzzle from the first clue without knowledge of the source language. Include alternative correct spellings where the design allows them.

### Review changes in content batches

Track source revisions and translation status so reviewers can see what changed between playable builds. Invite language-specific feedback on released text and give reviewers an ID, screenshot, and build number to tie reports to the right entry. Allcorrect's LocWorld session describes project management for crowdsourced localization. Treat such feedback as input to a named review owner. [Ksenia Mezhenina, LocWorld](https://allcorrectgames.com/talk-crowdsourced-localization-locworld/)

**Check** a late line change, an obsolete translation, and an unavailable localized voice asset. Confirm the release build follows its fallback policy, flags stale content for review, and binds the correct text and audio to the same scene.
