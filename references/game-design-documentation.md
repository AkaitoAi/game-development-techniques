# Game design documentation

## Problem and design reasoning

A shared idea becomes ambiguous when designers, engineers, artists, and testers translate it into separate tasks. A document is useful when it settles what players can observe, which rules govern edge cases, and who can decide an open question. Stone Librande describes one-page designs as a way to communicate more concisely. Warren Spector describes cutting the Deus Ex design document after the team understood what the game needed. A practical inference is to give each decision a compact home and attach deeper detail where implementation depends on it. [Librande, One-Page Designs, GDC 2010](https://gdcvault.com/play/1012356/One-Page) · [Spector, Deus Ex postmortem](https://www.gamedeveloper.com/design/postmortem-ion-storm-s-i-deus-ex-i-)

## Shipped examples

**Deus Ex (2000).** Spector describes narrowing the original design during development as the team found what worked on screen. The original ambition for realism shifted when playable work revealed different priorities. His written and conference postmortems provide a way to compare early intent against the released game. [Spector’s written postmortem](https://www.gamedeveloper.com/design/postmortem-ion-storm-s-i-deus-ex-i-) · [Spector, Classic Game Postmortem: Deus Ex, GDC 2017](https://www.youtube.com/watch?v=tffX3VljTtI)

**SimCity (2013).** Librande describes spending three years documenting the game through one-page designs and discusses advantages and drawbacks on a large project. Sheets can make a relationship visible at a glance. As an application, use a diagram when the shape of the system matters and attach a rule table when implementers need exact outcomes. [Librande, Simulating a City, One Page at a Time, GDC 2013](https://www.gdcvault.com/play/1017708/Simulating-a-City-One-Page)

## Techniques and practical checks

### Define the feature in player-visible terms

Write the intended player decision, what information is available before it, and the visible result. Include scope boundaries and any dependencies. Link detailed rule tables or state diagrams from this overview.

**Check** by having engineering, art, and QA independently describe the same scenario. Resolve disagreements about what the player sees, what they can do, and what persists.

### Record rules with concrete examples

For a consequential rule, state preconditions, outcome, order of evaluation, interruption behavior, and failure feedback. Include a worked example with starting state, input, and expected result.

**Check** one normal case, one boundary case, and one interaction between rules. Turn the examples into acceptance tests or a repeatable manual fixture.

### Give rules and tuning values a single owner

Keep stable design intent and invariants in the guide. Link rapidly changing values to their authored data and identify the person or team that resolves decision conflicts. Mark unresolved questions with the evidence needed to settle them.

**Check** by changing a tuning value through the normal authoring path. Confirm that the guide directs the reader to the correct current value and that affected tests still express the intended behavior.

### Reconcile the document with playable changes

Update the decision record when a playable test changes a requirement. Preserve a short reason for changes that affect other features and link the relevant test result or build.

**Check** after a cut or redesign by tracing the change through rules, UI, content, and tests. Ask a collaborator to explain what they would implement from the current guide and compare it with the build.
