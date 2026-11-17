# Input rebinding

## Problem and design reasoning

Games can offer interchangeable controls only when their actions have stable meanings independent of the physical input. Rebinding also creates a configuration problem because overlapping contexts, stale prompts, and inaccessible recovery screens can undo a working layout. Naughty Dog explicitly describes remapping and alternatives to holds and rapid presses as ways to address the difficulty of certain input gestures. A transferable design is to store physical bindings and activation behavior independently, with contextual resolution between them. [Naughty Dog's accessibility guide](https://blog.playstation.com/2020/06/09/the-last-of-us-part-ii-accessibility-features-detailed/)

## Shipped examples

- **Quake (1996).** The released source maps keys to commands and writes active bindings into configuration. Separate menu, console, and gameplay handling shows why the same key can require context-sensitive treatment. [id Software source](https://github.com/id-Software/Quake/blob/master/WinQuake/keys.c)
- **The Last of Us Part II (2020).** Naughty Dog provides custom controller layouts, alternate orientations, and ways to change holds into toggles or repeated taps into holds. The 2025 PC release added primary and secondary keyboard and mouse bindings and a mode for combined keyboard and controller input. [Naughty Dog console guide](https://blog.playstation.com/2020/06/09/the-last-of-us-part-ii-accessibility-features-detailed/) · [Naughty Dog PC overview](https://blog.playstation.com/2025/03/20/the-last-of-us-part-ii-remastered-pc-features-and-new-no-return-content-detailed/)
- **Developer conference reference.** The GDC 2021 session on mobility considerations describes supporting alternative devices, in-game assists, and simplified controls. Those considerations help define a rebinding test matrix for players who use several input methods. [GDC mobility session](https://www.gdcvault.com/play/1027111/Accessibility-Best-Practices-Mobility)

## Techniques and practical checks

### Map controls to stable action identifiers

Use action identifiers for game logic and store per-device bindings against those identifiers. Define which contexts may be active together, such as gameplay and a modal menu. Distinguish harmless reuse across mutually exclusive contexts from conflicts that would dispatch two actions.

**Check** assigning one input to two mutually exclusive actions, then assigning it to two actions that can run together. Verify the first works and the second gives a clear replace, swap, or cancel choice.

### Capture a replacement binding without losing navigation

Make capture a modal state. Let the input that opens capture return to neutral before listening. Support cancellation through a known route, present a preview, and keep the current binding active until the change is accepted. Preserve a usable path to reset controls if an essential action becomes unavailable.

**Check** opening capture while holding a key, accidentally bumping a stick, disconnecting the device, and attempting to clear the last menu-confirm binding. Verify that every case still allows recovery.

### Use resolved bindings for prompts and activation

Generate glyphs and text from the active context and the device whose input the prompt is meant to describe. Process gestures such as hold, toggle, and repeated press in an activation layer. Clear or transition active gesture state when focus changes or a device disconnects.

**Check** switching devices while one action is held, then opening a tutorial and a settings menu. Confirm there are no stuck actions and that displayed prompts match usable bindings.

### Save overrides and test migrations

Save custom mappings against stable action and device identifiers. Apply them over defaults and handle actions introduced, renamed, or removed by a patch. Mark controls that cannot be restored and let the player review or reset affected mappings.

**Check** restarting with custom bindings, importing an old profile, and connecting a device with a different layout. Confirm valid overrides persist independently of the removed or unavailable inputs.
