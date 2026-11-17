# Environmental storytelling

## Problem and design reasoning

A room can look convincing and still tell the player little about who used it or what happened there. Environmental storytelling makes evidence in the space support a specific inference. [Level design](level-design.md) covers route and encounter structure, while [Quests and narrative](quests-and-narrative.md) covers progression and explicit narrative state.

Fullbright's developers described wanting Gone Home's house to feel inhabited and exploratory even though the main story had a sequence. Their documented aims were experiential. A transferable inference is that a setting's contradictions and personal traces can reward investigation without requiring every object to carry a plot beat. [Kate Craig and Steve Gaynor, The Level Design of Gone Home, GDC 2015](https://media.gdcvault.com/gdc2015/presentations/Craig_Kate_LevelDesignOf.pdf)

## Shipped examples

- **BioShock (2007).** Smith and Worch's presentation analyzes the traces of a New Year's party in Rapture. The remaining decoration suggests the kind of celebration that preceded the collapse, with recorded dialogue supplying additional context. This is the presenters' reading of a shipped scene. [Harvey Smith and Matthias Worch, What Happened Here?, GDC 2010](https://media.gdcvault.com/gdc10/slides/Smith_Harvey_WhatHappenedHereWeb_Notes.pdf)
- **Gone Home (2013).** Fullbright used household space and decoration to make the family feel as if they occupied the house before the player entered. The larger environment invites exploration around a more structured story. [The Level Design of Gone Home, GDC 2015](https://www.gdcvault.com/play/1022112/Level-Design-in-a-Day)
- **Tacoma (2017).** Fullbright's workshop describes designing story segments to support self-directed discovery while staying grounded in physical space. The local order in which players encounter information is part of the design problem. [Steve Gaynor and Nina Freeman, Designing for Non-Linear Story Discovery in Tacoma, GDC 2018](https://www.gdcvault.com/play/1025178/Level-Design-Workshop-Designing-for)

## Practical techniques

### Author the inference before dressing the scene

Write one sentence about what happened, one about what the player can reasonably infer, and list the visible evidence. Decide which details establish the ordinary use of the space and which show a meaningful change.

**Check** show the graybox and evidence props to an unfamiliar player without narration. Ask what they think happened and which details led them to that reading. Revise evidence that consistently supports a different interpretation.

### Make evidence survive plausible discovery orders

Map clues to the facts they support. Give critical context an independent route and leave optional clues to reinforce or complicate what the player already knows. Preserve continuity when a scene can be revisited in a changed state.

**Check** enter from each likely approach, skip an optional room, and inspect clues in reverse order. Ask players to summarize the critical event and identify what remains uncertain.

### Separate decorative and story-critical signals

Use composition, contrast, sound, readable scale, or spatial grouping to give essential clues more presence than incidental props. Choose a second cue when visibility depends on optional audio, color discrimination, or camera placement.

**Check** at normal play speed and lowest supported presentation quality. Log which clues are noticed unprompted. Repeat with muted audio and from a different approach to see whether the scene remains interpretable.

### Keep state changes legible on return

Where gameplay or story changes a location, preserve a visible before-and-after relationship. Keep recognizable landmarks and alter a few signals that identify the changed state, such as a cleared shelf, interrupted work, or objects moved to a new owner.

**Check** visit before and after the change, then load a save directly into the later state. Confirm that returning players recognize what changed and first-time visitors can still infer the state needed to proceed.
