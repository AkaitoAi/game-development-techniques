# Contextual interaction

## Problem and design reasoning

A single input can mean talk, open, pick up, or attach depending on the player's target and current state. When several targets overlap, the wrong object can respond. When prompts and execution use different rules, the interface can offer an action the game refuses. Inferred design trade-off. A shared action reduces binding complexity, while a stable preview and explicit confirmation protect the player's intent when a choice matters.

## Shipped examples

**The Legend of Zelda: Ocarina of Time 3D (2011).** Nintendo's guide shows that the Action Icon reports different verbs, including Open, Talk, and Check. Targeting can also let Link converse with somebody at a distance. The documentation establishes the available behavior. The binding simplification is an inference from the control layout. [Nintendo quick guide](https://www.nintendo.com/eu/media/downloads/games_8/quick_start_guide/QuickStartGuide_3DS_TheLegendOfZeldaOcarinaOfTime3D_EN.pdf)

**The Legend of Zelda: Tears of the Kingdom (2023).** Ultrahand supports selecting, rotating, attaching, and detaching objects. The developers described using a visible adhesive joint so players could tell which objects were connected. They also wanted a consistent sound to communicate the repeated act of attaching objects. [Nintendo developer interview](https://www.nintendo.com/en-ca/whatsnew/ask-the-developer-vol-9-the-legend-of-zelda-tears-of-the-kingdom-part-4/) · [Nintendo gameplay guide](https://www.nintendo.com/en-gb/News/2023/May/12-spoiler-free-tips-for-your-The-Legend-of-Zelda-Tears-of-the-Kingdom-adventure-2385024.html) · [Nintendo's GDC presentation](https://gdcvault.com/play/1034667/Tunes-of-the-Kingdom-Evolving)

## Techniques and practical checks

### Decide which action is available

Give each candidate an action ID, target ID, eligibility rule, and prompt. Filter by state, range, and visibility, then resolve eligible choices using explicit priorities. Separate a disabled action that deserves explanation from one that should disappear entirely.

**Check** with a door, pickup, and character in the same small area. Vary distance, camera angle, inventory capacity, and the door's lock state. Verify that the visible prompt and action result agree in every case.

### Stabilize selection

Keep the current target until it stops being eligible or a challenger wins by a meaningful margin. Define a stable tie-breaker. For two useful actions on the same target, offer a secondary action or short choice list where selecting the wrong one would be costly.

**Check** sweep the view slowly across overlapping targets, then jitter around the crossover. Log target changes and repeat with camera and character pointing in different directions. Check that the target stays predictable and both actions remain reachable.

### Revalidate at commitment

Associate the input with the displayed action and target. Recheck them before changing world state, especially for holds and animated interactions. Define what happens if the target moves or another player claims it. Make cancellation return the player to control.

**Check** approach a consumable, begin interacting, then let another player take it or move out of range. Verify that the action cancels once, preserves unrelated state, and avoids applying the input to a replacement target.

### Communicate preview, result, and recovery

Show a preview appropriate to the interaction, such as the selected target or proposed attachment points. Make the completed state visible and distinguish success, blocked, and cancelled outcomes. Offer an undo or a clear reversal where experimentation is expected.

**Check** attach an object from two camera angles, detach just one joint in a group, and repeat with sound muted. Test that a player can identify the selected part, confirm the result, and recover from an accidental attachment.
