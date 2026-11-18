# UI settings preview design

## Problem and design reasoning

Setting labels often fail to communicate the experience they change. A player may need to hear a cue, read a representative subtitle, or try an aiming adjustment before deciding. This guide covers demonstrations and temporary preview state. Use settings and presets for grouping options and defining preset behavior.

The Last of Us Part I includes an Audio Cue Glossary where players can browse and listen to gameplay cues. Its accessibility volume controls distinguish cues, screen reader, dialogue, and other sound categories. This is a shipped example of making an unfamiliar option inspectable through its actual output. [Developer feature description](https://blog.playstation.com/2022/08/26/the-last-of-us-part-i-full-list-of-accessibility-features/)

Matthew Gallant's GAConf USA 2022 talk, **Expanding Accessibility in The Last of Us Part 1**, describes carrying accessibility features forward from Part II while developing further cinematic descriptions and DualSense features. The stated developer motivation was to go beyond feature parity. The preview architecture below is a portable recommendation, rather than a claim about Naughty Dog's internal menu implementation. [Developer conference session](https://www.gaconf.com/gaconf-usa-2022/)

## Techniques and practical checks

### Demonstrate the changed property

Choose a representative sample for each setting. Use real subtitle rendering for subtitle size, actual mixed cues for cue volume, and a controllable practice target for sensitivity. Keep unrelated properties constant during comparison. Explain when a demonstration cannot reproduce the live environment.

**Check** Change one option through its full range. Verify the preview changes the same property as gameplay, including supported language, text scale, output device, and display aspect ratio.

### Define the preview transaction

Represent saved values, pending choices, and temporary output explicitly. State whether changes apply immediately, require confirmation, or require restart. On cancel, restore the documented baseline. A sound sample should stop when its panel closes or another sample replaces it.

**Check** Change several values, cancel, reopen, apply, and restart. Verify displayed controls, runtime values, and persisted values agree with each action's declared behavior.

### Keep the preview operable with its own settings

Ensure narration, focus visibility, captions, and volume controls remain reachable while testing them. Provide a way to replay a sample. Do not require hearing a sound to locate its caption option or reading small text to recover readable text size.

**Check** Operate the preview with sound muted, a screen reader, keyboard or controller only, and maximum text size. Confirm focus stays visible and recovery controls remain accessible.

### Check representative conditions and cost

Include quiet and dense samples when masking matters. For graphics previews, distinguish visual demonstration from measured device performance. Label a synthetic example and validate settings in actual gameplay before claiming a frame-rate improvement.

**Check** Compare the preview with a dense gameplay scene on a target device. Record perceptual mismatches, preview resource use, and whether asynchronous loading or sample playback causes a menu hitch.
