# Reward reveal design

## Problem and design reasoning

A reward screen can celebrate a result while leaving the player unsure what was earned, what changed in their collection, or whether they can leave safely. This guide covers presentation after reward resolution. [Loot generation and drop tables](loot-generation-and-drop-tables.md) covers how rewards are selected.

In shipped *Hearthstone*, Blizzard introduced Catch-Up Packs because new and returning players were concerned about rebuilding collections. The feature supports opening several packs together. It demonstrates a delivery format that accommodates many rewards and a particular player need. Derek Sakamoto's GDC 2015 talk covers the vision and process behind Hearthstone's immersive UI. The reveal states and checks below are portable recommendations, and the talk's public description does not establish a particular transaction protocol. [Blizzard, Introducing Catch-Up Packs](https://hearthstone.blizzard.com/en-us/news/24008690/introducing-catch-up-packs) · [Derek Sakamoto, Hearthstone: How to Create an Immersive User Interface, GDC 2015](https://gdcvault.com/play/1022036/Hearthstone-How-to-Create-an)

## Techniques and practical checks

### Keep the grant and its reveal distinct

Have presentation read a committed reward result with a stable occurrence identity. Record what the player has already seen only where the experience needs it. Define whether replaying a presentation also replays sounds and celebrations, while preserving one grant.

**Check** close the reward screen, terminate the app during a reveal, and reopen from the same saved state. Compare inventory and progression before and after. Verify the reward appears once and an unseen result remains inspectable.

### Communicate significance through consistent cues

Choose a compact hierarchy for quantity, identity, novelty, and any special quality. Use typography, shape, iconography, and optional audio alongside color. Match emphasis to what the player can usefully understand, and keep the item description available.

**Check** ask a first-time player to identify the item, quantity, and whether it changes their available choices. Repeat with sound muted and color-dependent cues reduced. Record incorrect interpretations.

### Support both deliberate reveals and bulk review

Allow players to advance or skip presentation when the game permits, then provide a legible summary for large batches. Preserve item inspection and focus when switching between a sequence and a summary. Keep acknowledgment separate from equipping or spending.

**Check** reveal one reward, several distinct rewards, and many duplicates. Skip midway, inspect one item, and return. Verify all results remain accessible and no default input accidentally equips or spends a reward.

### Return players to the relevant next action

Offer a clear route back to play or to the system where the reward can be used. Explain unavailable actions in place, such as an item that requires a later unlock. Preserve the player's previous navigation context when the reveal interrupts another flow.

**Check** trigger the reveal from a match result, an inventory action, and a login event where supported. Dismiss it with each supported input device and confirm the resulting screen has the intended selection and usable controls.
