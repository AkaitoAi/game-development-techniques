# In-game editor design

## Problem and design reasoning

Players creating inside a game need to discover the tools, understand which state is being edited, and see the effect of a change quickly. A powerful editor can stall a first session with mode confusion or a long trip from placement to play. The cases below support an editing loop where players make a small change, try it in gameplay, and return to the same selection without losing their work.

**Super Mario Maker 2.** Nintendo presents Course Maker as a place where players can create courses and test them directly. Nintendo also provides tutorials and supports two people editing a course together on a single screen. A transferable inference is that testing should be easy even when the author is still learning the available objects and rules. [Nintendo, Make](https://supermariomaker.nintendo.com/make/) · [Nintendo, Play](https://supermariomaker.nintendo.com/play/) · [Nintendo, Multiplayer](https://supermariomaker.nintendo.com/multiplayer/)

**Dreams.** Sony and Media Molecule's GDC 2021 UX presentation describes using user research to adapt studio tools into a public creation experience. They discuss how feedback influenced UI, controls, tutorials, and onboarding. The documented design goal was enabling a broader audience to create with tools that had previously been used by developers inside the studio. [GDC 2021, Expanding the Dreamiverse: Making Dreams an Experience for Everyone](https://www.gdcvault.com/play/1027293/UX-Summit-Expanding-the-Dreamiverse)

## Techniques and practical checks

### Preserve editor context through playtesting

Use an isolated playable instance and retain the editing camera, active tool, and selection. Provide explicit actions for any runtime changes that should become authoring changes.

**Check** place an object, play from a point near it, change game state, then return to editing. Verify that test state is discarded as intended and the author resumes from the previous view and selection.

### Make edits reversible at the action level

Represent a drag, paint stroke, or multi-selection edit as one undoable operation. Keep links between objects consistent when the operation is undone or redone.

**Check** create a linked pair, move both in one gesture, delete one, then undo and redo the sequence. Compare positions, references, IDs, and selection with the expected state after every step.

### Show constraints at the point of interaction

Preview placement, snapping, and invalid positions before committing a change. Keep the active tool and selection visible. Teach tricky constraints with an editable example the player can try immediately.

**Check** observe new creators placing a connector, overlapping an occupied tile, rotating an object, and switching input devices. Record misplacements, selection mistakes, and time to recover without coaching.

### Separate drafts from validated versions

Autosave recoverable drafts and run targeted checks on the exact revision entering the next stage. Report issues at the offending object or location. Invalidate prior test results when relevant content changes.

**Check** close the game during a draft edit, reload it, and repair a missing start or finish point using the validator. Then edit a previously validated route and confirm that stale validation is cleared.
