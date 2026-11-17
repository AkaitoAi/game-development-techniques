# Mobile first-session design

## Shipped examples

**Prince of Persia: The Lost Crown (mobile, 2025).** Ubisoft's free trial starts at the beginning of the game and preserves trial progress when the full game is purchased. Ubisoft Da Nang's GDC 2026 case study shows first-time user experience revisions built around control presets, including a recommended touch profile and a separate assisted profile. Players can revisit those choices in settings. [Ubisoft mobile release](https://www.ubisoft.com/en-sg/game/prince-of-persia/the-lost-crown/news-updates/6jZ5ewg9gbmRzKEvAVBbUX/prince-of-persia-the-lost-crown-now-available-on-mobile) · [Alexis Denancé, GDC 2026 slides](https://media.gdcvault.com/gdc2026/Slides/GDC_POPTLC_UX%2BMOBILE%2BPORTING%2BCHALLENGES_PDFVERSION.pdf)

**Clash of Clans (October 2026 update).** Supercell's shipped UI overhaul gives players a short first-login walkthrough to locate the redesigned Build Menu. Several other lessons remain optional after they return to the village. This example concerns the first session after a major interface update. [Supercell interface redesign notes](https://supercell.com/en/games/clashofclans/blog/news/weve-revamped-the-clash-of-clans-user-interface/)

## Problem and design reasoning

A first-time player may have a short window, a muted phone, an unfamiliar touch layout, and a slow connection. When every setup task appears at launch, the player may leave before learning what they can do. Ubisoft describes addressing the challenge of preserving a console platformer's depth while making touch play and optional assistance usable. Supercell identifies a crowded interface that obscured what to do and concentrates its initial walkthrough on one changed action. A transferable inference is to distinguish essential first-session decisions from choices that can wait until the player has context, while keeping a visible route back to any setting deferred at launch. [Ubisoft accessibility interview](https://news.ubisoft.com/pl-pl/article/7HvHq2SIDTcXg6ap0v4QgD/prince-of-persia-the-lost-crown-mobile-accessibility-spotlight) · [Supercell interface redesign notes](https://supercell.com/en/games/clashofclans/blog/news/weve-revamped-the-clash-of-clans-user-interface/)

## Techniques and practical checks

### Make the first meaningful action reachable

Map launch to the first action that demonstrates the core loop and the first legible success. Mark mandatory install, authentication, controls, and accessibility steps separately. Defer optional account linking and notification prompts until players understand their value.

**Check** a fresh install on the lowest supported device and a constrained connection. Measure time to interactive play and first success separately. Record where a player who declines optional setup lands.

### Offer control choices in context

Provide a workable touch default and let players try the relevant movement or action before committing to a layout. Keep assist settings and feedback adjustments reachable from play. Communicate mandatory actions through visuals or text when audio is muted.

**Check** cold launch with sound muted, a touch-only device, and a controller attached. Disconnect the controller during the first exercise. Confirm that prompts and available actions update and the exercise can still be completed.

### Make tutorial checkpoints resumable

Persist the smallest state that identifies a completed step and any resulting reward. On resume, restore a playable state with the next instruction. Revalidate external actions, such as a sign-in, when the app regains focus.

**Check** background, suspend, and terminate at each tutorial boundary, including immediately after a reward. Resume and verify that steps remain completable and rewards appear once.

### Measure learning at the first free decision

Record tutorial outcomes and time between meaningful milestones alongside qualitative evidence. Define success as a player applying the mechanic independently after the prompt disappears. Test revised onboarding with both new and returning players.

**Check** observe fresh players in the first unguided challenge, and ask returning players to find the updated action they previously knew. Record independent completion, repeated failed inputs, use of help, and exits before and after the revision.
