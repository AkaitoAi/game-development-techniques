# Interactive music stingers

## Problem and design reasoning

A short musical accent can mark discovery, danger, failure, or success, but overlapping accents can obscure the result or clash with the score. Composer Winifred Phillips's GDC presentation draws on her work in *Assassin's Creed Liberation*, *God of War*, and *LittleBigPlanet* to discuss themes across gameplay contexts, including stingers. It establishes a compositional reason to derive short cues from the game's musical identity. It does not establish the exact scheduling implementation used in each title. [Winifred Phillips, From Assassin's Creed to The Dark Eye: The Importance of Themes, GDC 2020](https://www.gdcvault.com/play/1026747/contactUs)

Dan Golding's account of *Untitled Goose Game* describes recorded piano performances responding to screen events within a bounded delay, drawing on silent-film accompaniment. This is related evidence for matching musical response time to an event's expressive purpose. The cue arbitration, occurrence IDs, and timing contracts below are implementation recommendations. [Dan Golding, Finding Untitled Goose Game's Dynamic Music in the World of Silent Cinema, DiGRA Australia 2020](https://digraa.org/wp-content/uploads/2020/01/DiGRAA_2020_paper_47.pdf)

## Techniques and practical checks

### Define the event and its audible deadline

Specify which committed gameplay event requests the accent, whether it needs immediate onset or a musical boundary, and how late it can remain useful. Choose an asset family compatible with the underlying score or author a transition that creates room for it. Measure onset on the audio timeline.

**Check** issue a cue just before and after the intended beat boundary and during a frame hitch. Compare requested event time with captured audio onset and verify the cue meets its declared delay tolerance.

### Arbitrate simultaneous requests

Give each cue category an explicit priority and a rule for replacement, queuing, or combining. Consider the meaning of victory and danger arriving together. Limit repetitive accents per event family while retaining urgent gameplay information through other cues.

**Check** request discovery, damage, and encounter completion on one tick, then repeat one event rapidly. Confirm the chosen cue matches the intended meaning and queued cues expire when they are no longer relevant.

### Manage occurrence and owner lifetimes

Identify a cue by its logical event occurrence where replay or duplicate delivery is possible. Decide whether a pending cue survives scene changes, cancellation of its gameplay event, or checkpoint restoration. Release queued assets when their request retires.

**Check** restore before an event, receive a duplicate confirmation, and leave the scene before a scheduled onset. Verify audible delivery counts and ensure a cue from the old encounter cannot play in the new one.

### Mix accents with the gameplay context

Author headroom, duration, and spectral placement so the accent supports rather than hides important dialogue or warnings. Tune any ducking against actual overlapping content and offer the same essential information outside music when players reduce its volume.

**Check** audition each cue with the busiest supported score layer, a dialogue line, and an important warning. Repeat with music at zero and verify gameplay outcomes remain understandable through their other feedback.

See [Adaptive music](adaptive-music.md) for score structure and [Audio event lifetime management](audio-event-lifetime-management.md) for runtime ownership.
