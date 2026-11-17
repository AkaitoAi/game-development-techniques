# Mobile accessibility and one-handed play

## Problem and design reasoning

A conventional virtual controller can demand simultaneous input, wide thumb travel, and a stable two-handed grip. Nintendo designed *Super Mario Run* around one-handed play to make it practical in everyday mobile settings. *Afterplace* creator Evan Kice describes designing around native touch interaction instead of drawing controller buttons. An inference for another game is to choose which decisions require precise input, then simplify the surrounding gestures without removing that decision. [Nintendo announcement](https://www.nintendo.co.jp/corporate/release/en/2016/160908.html) · [Afterplace developer interview](https://developer.apple.com/news/?id=3exkgv43)

## Shipped examples

- *Super Mario Run* uses automatic running and touch-timed jumps in its mobile control design. The player directs key moments using a single hand. [Nintendo game site](https://supermariorun.com/en/) · [Nintendo announcement](https://www.nintendo.co.jp/corporate/release/en/2016/160908.html)
- *Afterplace* supports movement and attacks by touching anywhere and can adapt if the player uses two thumbs. Direct object taps can also trigger interaction. [Evan Kice's shipped game page](https://evankice.itch.io/afterplace)

## Techniques and practical checks

### Define a complete one-handed action path

List required actions across tutorial, moment-to-moment play, failure, and pause. For actions that ordinarily require simultaneous inputs, consider a contextual action, toggle, or movement assist. Retain bespoke two-handed controls if they serve a different play preference. Apple demonstrates flexible touch layouts and fluid interactions in its WWDC session. [Make your game great with touch, WWDC26](https://developer.apple.com/videos/play/wwdc2026/358/)

**Check** finish the shortest full gameplay loop using only the left hand, then only the right. Include recovery from a mistake, an interaction prompt, and pause and resume. Log actions that require a second hand.

### Make reach adjustable around a real grip

Offer mirrored layouts or movable action clusters when controls are fixed. Preserve spacing between controls with different consequences. Keep critical actions outside operating-system gesture regions and away from where the gripping thumb obscures near-term feedback.

**Check** play on the smallest and largest supported phones using both grips. Count grip changes, accidental presses, and moments where fingers obscure a cue. Confirm the customized layout survives restart and orientation changes where supported.

### Provide alternatives to high-demand gestures

Where an essential action needs a hold, repeated taps, or a precise swipe, define an equivalent control such as a toggle, an adjustable timing window, or a deliberate button. Keep menu actions accessible independently of the in-game gesture scheme.

**Check** complete an action with the original and alternative controls. Test slow gestures, involuntary second touches, interrupted holds, and leaving the gameplay screen while an input is active. Confirm neither mode triggers an unintended action.

### Validate mobile assistive feedback and discovery

Expose reachable, named controls for settings and menu navigation where the platform permits. Use optional visual, audio, and tactile feedback in combination so an action remains understandable when one channel is disabled. Make the one-handed preset available before it is needed.

**Check** navigate to the preset without a tutorial prompt. Restart, confirm it stays enabled, then test menus with the device's built-in accessibility tools and test gameplay once with audio muted and once with haptics disabled.
