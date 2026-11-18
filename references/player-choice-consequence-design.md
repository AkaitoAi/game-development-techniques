# Player choice consequence design

## Problem and design reasoning

An expensive branch can have little perceived value when players never recognize that their choice caused it. Discussing *The Witcher 2*, CD Projekt's Adam Badowski and Marcin Iwiński described the production difficulty of its divergent second act and the challenge of communicating alternate paths without spoiling immersion. Their documented concern connects branch cost to player understanding. [CD Projekt developer interview](https://www.gamedeveloper.com/design/what-i-the-witcher-i-taught-cd-projekt-about-rpgs)

Paweł Sasko's GDC talk, *10 Key Quest Design Lessons from The Witcher 3 and Cyberpunk 2077*, explicitly covers structuring choices and consequences alongside emotional impact and production effectiveness. The following workflow is a portable recommendation for making authored consequences understandable and consistent. [GDC session](https://gdcvault.com/play/1028897/10-Key-Quest-Design-Lessons)

## Techniques and practical checks

### Define the commitment

Record the player's intended action, known cost, uncertainty, cancellation point, and persistent outcome before writing dialogue or buttons. Communicate consequences the character could reasonably anticipate. Deliberate uncertainty should have a design purpose.

**Check** ask unfamiliar players what each option commits them to before they select it. Compare their expectations with the specification and investigate unintended surprises.

### Establish causal feedback

Provide an immediate acknowledgment of the decision. For delayed outcomes, include a contextual reminder that connects the event to the earlier action. Preserve ambiguity where the fiction needs it while retaining evidence players can interpret.

**Check** return players to a delayed consequence after intervening gameplay. Ask what caused it without supplying the answer and record mistaken explanations.

### Maintain one consequence state

Give outcomes stable identifiers and define which systems consume them. Dialogue, encounters, journals, rewards, and world changes should agree after travel, reload, and repeated interaction. Specify whether an outcome is reversible.

**Check** commit each branch, save, reload, revisit, and trigger dependent content in different orders. Compare resulting state and displayed acknowledgment.

### Budget observable differences

Estimate authoring and testing cost against the experience each branch adds. Track meaningful differences in relationships, resources, access, or interpretation, including consequences that eventually reconverge.

**Check** play alternate branches and identify their intended experiential difference. Verify reconvergence preserves established facts and does not silently erase promised consequences.
