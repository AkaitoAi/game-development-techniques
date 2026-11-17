# Mobile safe areas and orientation

## Problem and design reasoning

Mobile viewport dimensions and usable space can change independently. A notch can obscure a button without changing the aspect ratio, and a wide viewport can expose unfinished geometry or put a goal behind the HUD. At GDC 2026, the *Prince of Persia: The Lost Crown* mobile team documented these camera and HUD conflicts across phone aspect ratios and showed the trade-off between revealing a room and showing out-of-bounds content. An inference for other games is to treat world framing and input-safe layout as two rectangles with different requirements. [GDC 2026 porting slides](https://media.gdcvault.com/gdc2026/Slides/GDC_POPTLC_UX%2BMOBILE%2BPORTING%2BCHALLENGES_PDFVERSION.pdf)

## Shipped examples

- *Prince of Persia: The Lost Crown* mobile had to frame characters and geometry across several aspect ratios while accommodating its touch HUD. The porting talk compares camera constraints, adjustments to level composition, and HUD coverage. [Ubisoft developer slides, GDC 2026](https://media.gdcvault.com/gdc2026/Slides/GDC_POPTLC_UX%2BMOBILE%2BPORTING%2BCHALLENGES_PDFVERSION.pdf)
- *Vampire Survivors* exposes a setting to switch between portrait and landscape orientations. That demonstrates an explicit mode choice in shipped settings. The choice to persist an orientation preference is a design option to consider for other games. [poncle's App Store listing](https://apps.apple.com/us/app/vampire-survivors/id6444525702)

## Techniques and practical checks

### Separate the safe rectangle from decorative coverage

Draw backgrounds to the viewport edge where appropriate. Anchor essential buttons, text, prompts, and their hit regions within the platform-reported usable bounds. Keep left, right, top, and bottom insets independent, and recompute on geometry changes. [Design advanced games for Apple platforms, WWDC24](https://developer.apple.com/videos/play/wwdc2024/10085/)

**Check** both landscape rotations on a device with a cutout. Inspect the button edges, close controls, and joystick capture region. Repeat with the system gesture indicator visible.

### Specify minimum gameplay framing

Define what the player must see in every supported viewport. Fit or extend the camera around that requirement, and account for areas occupied by touch controls. For a fixed composition, use intentional letterboxing and verify the view-to-input transform excludes the bars.

**Check** the narrowest and widest supported shapes at an arena edge and in a camera transition. Mark the character, next obstacle, and interaction target. Confirm each remains readable and that no invalid world area is visible.

### Handle orientation as a layout transition

On an accepted rotation or window change, cancel stale pointers and recompute camera projection, insets, HUD geometry, and input mapping together. Keep session state separate from layout state. An explicit lock or preference can be useful when accidental rotation is disruptive. [Android large-screen game layout guidance](https://developer.android.com/games/develop/multiplatform/letterboxing)

**Check** rotate during a drag, during a modal, and during gameplay. Verify a canceled gesture triggers no game action and that the same visible object remains selectable after layout settles.

### Test the complete viewport matrix

Cover compact phones, large phones, and tablets. Include fold or split-window states where supported. Test layout at its real device density and with long translations and larger text.

**Check** run launch, tutorial, gameplay, pause, settings, and exit flows in each minimum-size layout. Log overlap with the safe bounds and verify every required control can be reached.
