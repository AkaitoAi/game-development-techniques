# Player failure feedback design

## Problem and design reasoning

A failed attempt can leave the player unable to identify what happened, what was lost, or what to try next. This guide covers the communication between a failure event and the next attempt. Use difficulty communication for setting expectations and game feel for moment-to-moment response.

Celeste's official changelog documents quick restart, fixes for incorrect respawn positions, and a timer rule change intended to remove an unfun retry exploit. These are concrete examples of failure handling involving input, state restoration, and result accuracy together. Matt Thorson's GDC 2017 talk, **Level Design Workshop: Designing 'Celeste'**, supplies the developer's level-design context. The diagnostic sequence below is a portable recommendation, rather than a claimed Celeste implementation. [Official changelog](https://www.celestegame.com/changelog.html), [GDC session](https://www.gdcvault.com/play/1024307/Level-Design-Workshop-Designing-Celeste)

Hades gives a different response to repeated failure. Supergiant describes God Mode as an optional damage-resilience feature that increases after deaths, motivated by opening roguelike experiences to more players. Its patch notes also record clearer Death Defiance presentation and a new lava screen effect. The lesson is to distinguish terminal defeat, recovery, and continuing damage before prescribing a response. [Developer FAQ](https://www.supergiantgames.com/blog/hades-faq/), [Developer patch notes](https://www.supergiantgames.com/blog/hades-welcome-to-hell-update-patch-notes/)

## Techniques and practical checks

### Preserve a trustworthy cause

Capture the resolved failure cause when authoritative gameplay commits it. Store source, relevant action, location, and outcome. If several hits arrive together, define a consistent explanation policy. Avoid blaming an enemy when a fall or scripted boundary actually ended the attempt.

**Check** Reproduce a simultaneous projectile hit and fall. Confirm feedback names the committed cause and matches recorded damage and death events.

### Offer one actionable observation

Show a relevant cue such as an attack direction, missed timing window, or depleted resource. Keep detailed breakdowns available on request. Reveal information according to the game's rules so feedback does not expose a hidden opponent or solve a puzzle accidentally.

**Check** Ask an unfamiliar tester what ended the attempt and what they would change. Record whether the explanation supports a concrete next action without developer prompting.

### Make retry an explicit state transition

Define when retry input becomes valid, what resets, and where control returns. Clear held attack or confirmation input where it could trigger an unintended action on respawn. Measure failure-to-control time separately from loading time.

**Check** Hold confirm through defeat, retry repeatedly, and change input device during transition. Verify a valid spawn, intended resource state, and no accidental immediate attack or second retry.

### Separate explanation from assistance

Offer optional practice or assistance after repeated failure with clear effects and player control. Preserve the distinction between a useful explanation and a change to the challenge. Keep attempt statistics accurate across retries, assists, and save reloads.

**Check** Fail repeatedly with assistance enabled and disabled. Confirm messages remain useful, offers can be dismissed, and completion records follow the declared rules.
