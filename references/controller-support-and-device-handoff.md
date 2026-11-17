# Controller support and device handoff

## Problem and design reasoning

Players can change device while a button is held, reconnect with a different device identity, or use an adapter that changes the reported controller family. Symptoms include stuck movement, doubled UI actions, prompts that flicker between device families, and a reconnect that assigns the controller to the wrong player. Define device ownership, action state, and prompt selection as separate policies. That separation is a design inference from the shipped behavior and fixes below.

## Shipped examples

- **God of War Ragnarök on PC (2024).** Santa Monica Studio documents several native controller families and Steam Input support. A patch corrected Switch controller prompts when Steam Input was used, while another made wireless DualSense use fallback rumble when advanced haptics were unavailable over that connection. These fixes illustrate why button artwork and optional device effects each need capability checks. [Supported controllers](https://support.sms.playstation.com/hc/en-us/articles/27152724494093-Supported-Controllers) · [Patch 7](https://support.sms.playstation.com/hc/en-us/articles/31917242451213-God-of-War-Ragnar%C3%B6k-PC-Patch-7) · [Patch 5](https://support.sms.playstation.com/hc/en-us/articles/30785912302349-God-of-War-Ragnarok-PC-Patch-5)
- **Prince of Persia: The Lost Crown on mobile (2025).** Ubisoft Da Nang shipped an adapted touch interface and external controller support. At GDC 2026, Alexis Denancé presented the team's approach to adapting controls and UX while preserving the original game experience. The reusable design choice is to let different physical interfaces express the same gameplay intent through layouts suited to each device. [Ubisoft mobile release announcement](https://news.ubisoft.com/en-gb/article/6btDjmzXD7EGEBYhkYmzcB/prince-of-persia-the-lost-crown-mobile-coming-to-mobile-devices-on-april-14) · [GDC 2026 session](https://schedule.gdconf.com/session/ux-mobile-porting-challenges-in-prince-of-persia-the-lost-crown-case-study/915038)

## Techniques and practical checks

### Normalize input with device capabilities

Translate hardware events into actions at the input boundary. Keep the runtime source and controller family available to UI and feedback. Use one input route for a physical controller when both a native backend and virtual adapter expose it, using platform pairing or an explicit user choice where attribution is uncertain.

**Check** a native controller, a controller routed through the supported translation layer, and a generic controller. Press confirm once on each and verify a single action, the correct prompt or a generic fallback, and graceful degradation if advanced feedback is unavailable.

### Resolve the active prompt from intentional input

Keep prompt changes independent of device connection. Use purposeful input such as a button press or motion past a dead zone and apply a small policy-defined hold period if devices compete. Support simultaneous inputs where gameplay calls for them.

**Check** mouse movement with an idle drifting stick, then switch deliberately between controller and mouse during a tutorial. Verify stable glyphs, correct binding display, and continued progress from either device.

### Define disconnect and reconnect transitions

Cancel held actions on device loss, clearing movement, repeat, and vibration state. Retain the player slot and profile long enough to reconnect, and require explicit selection where identity is ambiguous.

**Check** disconnect while moving, during a hold-to-confirm interaction, and while paused. Reconnect using another port or an equivalent controller and verify that the player resumes with the expected profile and neutral input state.

### Preserve local player ownership

Bind joining to an intentional action and tie devices to player slots. Define whether a secondary device can assist one slot or join another. Keep menu focus per player where independent interaction is supported.

**Check** join two controllers, disconnect player one, have player two operate the menu, then reconnect player one. Verify the original slots, focus, and per-player feedback remain correct.
