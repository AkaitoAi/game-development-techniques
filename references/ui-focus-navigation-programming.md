# UI focus and navigation programming

## Problem and design reasoning

Layered menus can leave a hidden widget handling input or lose selection when a list changes. The resulting failure appears as a dead controller, a double activation, or an action reaching gameplay through a modal. Epic documents CommonUI as technology originally developed for *Fortnite*, including selective input routing and controller navigation. Joel Gonzales's GDC 2023 talk describes the UI engineering architecture shipped in *Marvel's Midnight Suns* and the need to share behavior across many screens. A transferable inference is to make activation, focus ownership, and restoration an explicit contract of each screen. [Epic, CommonUI overview](https://dev.epicgames.com/documentation/unreal-engine/overview-of-advanced-multiplatform-user-interfaces-with-common-ui-for-unreal-engine) · [Joel Gonzales, UI Engineering Patterns from Marvel's Midnight Suns, GDC 2023](https://gdcvault.com/free/gdc-23/play/1028880/UI-Engineering-Patterns-from-Marvel)

## Techniques and practical checks

### Scope focus to the active player and layer

Track focus per local player and route an action through the highest eligible active layer. Define whether each layer consumes, passes, or blocks navigation, confirm, cancel, and gameplay input. Activate a screen before choosing its initial focus target.

**Check** open a modal from gameplay while a direction or confirm button is held. Verify the opening press cannot also accept the modal and the blocked gameplay action does not execute.

### Restore focus using logical identity

Store a stable item or action identity when leaving a screen. On return, resolve it against current content and choose a documented neighboring or default target if it disappeared. Treat recycled visual list cells as temporary views.

**Check** select an inventory item, open its details, remove that item through a valid game event, and return. Also reorder the list while scrolled. Verify the selected item and visible highlight refer to the same logical object.

### Define navigation edges and fallbacks

Use authored edges for irregular layouts and a consistent geometric policy for generated content. Exclude hidden, disabled, and unavailable targets according to the screen's policy. Specify wrap behavior and ensure required actions remain reachable.

**Check** traverse the complete screen using only the supported keyboard or gamepad controls at several aspect ratios and text sizes. Hide an intermediate item and verify navigation reaches a legal target without trapping focus.

### Trace ownership across device changes

Keep pointer hover separate from controller selection when both exist. Record activation, focus movement, and input-consumption decisions in a small diagnostic trace. Update prompts from the actual active input context.

**Check** switch between mouse, keyboard, and gamepad while a dialog is open, then disconnect and reconnect a controller. Verify one selected target receives each action and cancel returns to a usable screen.
