# Adaptive music

## Shipped examples

**Doom (2016).** Composer Mick Gordon's conference talk describes a high-energy soundtrack designed to support gameplay and stay true to the franchise. It covers the intersection of composition, interactive music, synthesis, and mixing. [Mick Gordon, GDC 2017, Doom: Behind the Music](https://www.gdcvault.com/play/1024068/DOOM-Behind-the)

**Untitled Goose Game (2019).** Composer Dan Golding describes designing the score around the style of a silent-film accompanist. The arrangement uses short pieces of recorded piano performance and variations that let the game respond to what is happening while keeping coherent musical phrasing. [Dan Golding, DiGRA Australia 2020 conference paper](https://digraa.org/wp-content/uploads/2020/01/DiGRAA_2020_paper_47.pdf) · [Conference program](https://digraa.org/post/digra-australia-2020-conference-program/)

## Problem and design reasoning

Players set the duration and sequence of play, so a fixed soundtrack can miss a change in mood or react at the wrong musical moment. Golding describes a score that follows comedic activity while retaining the character of a piano performance. Gordon's talk frames musical identity and gameplay support as joint goals. The implementation inference is to choose response timing and musical granularity around the cue's purpose. A surprise may need a quick accent, while building tension can use a slower transition that preserves a phrase.

## Techniques and practical checks

### Map gameplay states to musical decisions

Give the score a small state model with explicit priority, entry and exit conditions, and optional hold times. Express transitions as player-facing changes such as safety, rising activity, or resolution. Send committed events to the score and use hysteresis for noisy signals.

**Check** repeatedly cross a state boundary, trigger two competing events in the same frame, and hold an encounter at its threshold. Log requested and committed states and confirm that the score responds once per meaningful change.

### Schedule transitions on the audio clock

Define the earliest acceptable transition and the maximum acceptable delay for each cue. Schedule aligned changes using the audio engine's clock or musical transport. When the next musical boundary would arrive too late, choose an approved short accent or transitional segment.

**Check** issue the same state request just before and after a beat or bar boundary and repeat during a frame hitch. Record the actual audio onset and verify that every cue meets its timing tolerance without clicks or rhythmic drift.

### Choose layers or sections based on what changes

Use phase-aligned layers when intensity changes while the underlying composition remains compatible. Switch whole sections when the harmony, tempo, or structure needs to change. Author permitted entry and exit points and ensure that optional layers and stingers leave enough headroom for key gameplay sounds.

**Check** audition every permitted pairing in a packaged build and interrupt a transition with a higher-priority cue. Listen for harmony clashes, transient doubling, loop gaps, and masking of dialogue or warnings.

### Exercise long and interrupted sessions

Specify fallback cues if an asset or segment is unavailable. Define whether pause, checkpoints, and scene transitions resume musical time, seek to a new position, or restart the score. Give sustained states enough variation to support players who linger.

**Check** idle longer than the expected loop cycle, pause and resume during a pending cue, then reload a checkpoint mid-section. Verify state recovery, absence of duplicated or stale cues, and continuity appropriate to each operation.
