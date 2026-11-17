# Mobile input latency

## Problem and design reasoning

Touch responsiveness depends on sampling, gesture recognition, simulation, rendering, and display. Epic documents mobile touch controls and device preview for *Fortnite* islands, while Apple discusses touch support at WWDC. An inference for diagnosis is to measure the complete chain on a device before tuning one stage. [Epic, Designing for Mobile in Fortnite](https://dev.epicgames.com/documentation/fortnite/designing-for-mobile-in-fortnite) · [Apple WWDC26, Make your game great with touch](https://developer.apple.com/videos/play/wwdc2026/358/)

## Techniques and practical checks

### Instrument the input path

Record the event timestamp, recognition time, simulation consumption, and first affected frame.

**Check** Compare traces with a high-speed view of finger and display. Report a median and tail latency for a repeated action.

### Make gesture commitment deliberate

Choose which actions can begin on touch-down and which require movement or release to distinguish intent.

**Check** Compare quick taps, slow swipes and ambiguous drags. Count unintended actions as well as delay.

### Use an immediate acknowledgment where useful

Provide reversible visual feedback while an authoritative result is pending.

**Check** Under delayed responses, confirm acknowledgment appears promptly and reconciles cleanly when the action is refused.

### Exercise device conditions

Capture the same test across supported refresh modes, thermal states and dense scenes.

**Check** Compare which stage changes across conditions. Validate tuning on real hardware and use touch simulation for layout iteration. [Epic mobile preview guide](https://dev.epicgames.com/documentation/fortnite/mobile-preview-session-in-unreal-editor-for-fortnite?lang=en-US)
