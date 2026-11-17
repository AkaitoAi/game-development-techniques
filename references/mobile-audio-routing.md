# Mobile audio routing

## Problem and design reasoning

On mobile, the operating system can change audio focus, interrupt playback, or switch the physical output while gameplay continues. Treat these events independently. A Bluetooth disconnect can change the route while the game stays foregrounded, and an incoming call can interrupt audio while the route remains connected. Keep the player's sound preferences separate from these transient conditions. [Apple route changes](https://developer.apple.com/documentation/avfaudio/responding-to-audio-route-changes) · [Apple audio interruptions](https://developer.apple.com/documentation/AVFAudio/handling-audio-interruptions) · [Android audio focus](https://developer.android.com/media/optimize/audio-focus)

## Shipped example and developer talk

**Hearthstone mobile, Patch 26.0.** Blizzard support explained that an earlier restriction on concurrent audio was introduced to avoid mobile crashes. The patch narrowed the affected devices and restored background audio from other apps on iOS. A design inference is that other-app audio belongs in the device compatibility matrix alongside game playback and stability. [Blizzard support explanation](https://us.forums.blizzard.com/en/hearthstone/t/hearthstone-pauses-background-audio-on-mobile/104087/56)

At WWDC23, Apple's Suraj described AirPods switching between devices and hardware mute integration. The design implication is to use system-reported state as an input and reconcile in-game indicators with the active route and capture state. [Enhance your app’s audio experience with AirPods, WWDC23](https://developer.apple.com/videos/play/wwdc2023/10233/)

## Implementation and verification

1. **Separate focus from user settings.** Track user mute, foreground state, system focus, and route independently. Resume only when those inputs allow playback. **Check** start an external podcast, open the game with music off, then trigger a transient interruption. Confirm the mix follows the intended policy and game music stays disabled.
2. **Reconcile physical output changes.** Re-query the route after headset changes. When a private output disappears, apply the agreed speaker fallback policy before releasing queued audio. Reconfigure the audio engine if the actual stream format changes. **Check** disconnect and reconnect Bluetooth and wired headsets during menus and gameplay. Confirm there is no loud speaker burst, duplicate music, or permanent silence.
3. **Treat microphone use as a separate mode.** Request capture only after an explicit player action. Re-evaluate available input and output when entering or leaving voice chat. Mirror the effective mute state in the UI and restore the game-only audio mode after capture stops. **Check** deny microphone permission, then test a permitted voice session with headset disconnect and hardware mute. Verify that capture stops at the expected times and game audio recovers.
4. **Recover from overlapping events.** Serialize audio state updates and make suspend and resume idempotent. Resume loops at the correct state and discard stale one-shot effects. **Check** trigger a call, disconnect headphones during it, background the game, and return. Repeat with the player muted and while in a rhythm-sensitive segment. Verify one audio graph, correct mute behavior, and the intended timeline or explicit restart.
