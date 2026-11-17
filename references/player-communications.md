# Player communications

## Problem and design reasoning

**Problem.** Players miss time-sensitive callouts, cannot or prefer not to speak, or need to limit contact from other players. Test whether teammates can coordinate with voice off and whether privacy settings cover every channel.

**Design reasoning.** Rare documents several communication modes, while Respawn supplies contextual callouts through pings. The inferred trade-off is to make common team messages quick to send and reserve richer channels for messages that need detail. See the shipped examples below for sources.

Use this guide to design voice, text, and preset communication during multiplayer games. Define what players need to convey, to whom, and at which point in play.

## Shipped examples

**Sea of Thieves (2018).** Rare documents communication options including voice, text, presets, and transcription. Its accessibility panel and support documentation cover platform settings and in-game controls. The broader development process is covered in Rare's GAConf Europe 2026 accessibility presentation. [Rare accessibility guide](https://www.seaofthieves.com/accessibility) · [GAConf Europe 2026 program](https://www.gaconf.com/gaconf-europe-2026/)

**Apex Legends (2019).** Respawn's contextual ping system communicates with squadmates without requiring a keyboard or microphone. The Arsenal update extended map pings to interactable objects with accurate map waypoints and callouts. [EA's player guide](https://help.ea.com/en/articles/apex-legends/how-to-play/) · [Respawn's Arsenal patch notes](https://careers.ea.com/ea-play/news/arsenal-patch-notes)

## Techniques and practical checks

### Route by audience and context

Define who can send and receive on each channel, such as party, team, and nearby players. Label the active audience before a player speaks or sends. Re-evaluate permissions after a team change, reconnect, or spectator transition, and apply block, family, and platform settings to both delivery and transcription.

**Check** from two parties through a team swap and a reconnect. Confirm that messages and transcribed audio go only to the intended recipients and that the displayed audience matches the effective channel.

### Offer equivalent ways to coordinate

Design a small set of contextual signals for urgent tasks, such as requesting help or marking a destination. Represent signals with an icon and text or audio where appropriate. Offer transcription and narration options alongside voice and text, and give players a separate way to control volume and narration speed. [Xbox Accessibility Guideline 119](https://learn.microsoft.com/en-us/xbox/accessibility/xbox-accessibility-guidelines/119)

**Check** a complete team task with microphone disabled, then with audio muted and then using narration and transcription. Verify that each player can identify the sender, intent, location, and expiry of critical signals.

### Put safety controls where interactions happen

Place per-player mute, block, and report controls close to the roster or message. Make their effects clear. Apply relevant restrictions before delivering chat or its derivatives. Rate-limit free text and preset signals so a coordination tool remains usable during bursts.

**Check** muting during an active transmission, blocking before reconnect, and reporting from chat history using test accounts. Verify behavior in voice, text, pings, and narration, including after a change of match or platform.

### Minimize voice and transcript retention

Limit access to audio, transcripts, and reports by purpose and role. Prefer transient processing for routine chat when feasible. When evidence is retained for safety review, define what is captured, who can access it, and when it expires. Disclose those choices to players and prevent opt-outs or microphone permission changes from silently enabling capture.

**Check** with synthetic test audio and transcripts. Trace what is sent, retained, visible to reviewers, and purged. Verify that denied microphone access and push-to-talk release stop transmission and that reports retain only the intended evidence.
