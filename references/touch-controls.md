# Touch controls

## Problem and design reasoning

On a touchscreen, the same surface receives input and shows its result. A thumb can cover a target, a resting contact can drift, and players may have no tactile reference for the edge of a button. Choose the touch vocabulary around the decisions the player needs to make. A timing game may suit one repeated gesture. A game that needs simultaneous movement and aim needs clear ownership for multiple contacts.

**Documented reasoning.** Shigeru Miyamoto described one-handed play as an explicit goal for *Super Mario Run*. Supercell documented adding a joystick-center vibration in *Brawl Stars* to make cancellation easier. Zach Gage's GDC talk examines touch input through tactile feedback and the feel of the control itself. [Nintendo launch announcement](https://www.nintendo.com/en-za/News/2016/September/Super-Mario-Run-coming-to-iPhone-iPad-this-December-1138028.html) · [Supercell December update](https://supercell.com/en/games/brawlstars/blog/game-updates/december-update/) · [Zach Gage, Controls You Can Feel, GDC 2012](https://gdcvault.com/play/1015404/Controls-You-Can-Feel-Putting)

**Transferable inference.** Before adding an on-screen control, test whether the game can automate an operation with little decision value. When a gesture needs a cancel zone, make the transition legible while the finger is still down.

## Shipped examples

- **Super Mario Run (2016).** The character moves forward automatically, while the player times touches to jump. The player's finger can operate the action without staying over a specific virtual button throughout the course. [Nintendo game page](https://www.nintendo.com/en-gb/Games/Smart-device-games/Super-Mario-Run-1140496.html)
- **Brawl Stars (2018).** Its December update documented haptic feedback in the joystick's middle area for aim cancellation, and a later release documented improvements to joystick controls on tablets. [Supercell December update](https://supercell.com/en/games/brawlstars/blog/game-updates/december-update/) · [Supercell tablet control update](https://supercell.com/en/games/brawlstars/blog/release-notes/update-release-notes-2/)

## Techniques and practical checks

1. **Prototype the fewest gestures that preserve the decisions.** List the decisions made under time pressure, which can happen together, and which can be inferred from game state. Compare a screen-wide action with a fixed button when both can express the same intent. **Check** the same short level with both variants and both hands. Record missed actions, accidental actions, and moments when the finger hides important information.
2. **Give each contact an explicit owner.** Assign a touch ID to its control at press time, preserve ownership across drag, and release it on end or cancellation. Decide which modal UI states may claim new contacts and which must cancel active gameplay contacts. **Check** movement plus aim with two fingers, lifting them in both orders, sliding across regions, an OS interruption, and opening a modal during a hold. Verify the character never keeps moving once its controlling touch ends.
3. **Show gesture state before committing.** Expose an aim preview or drag target during movement. Define separate enter and exit thresholds where thumb jitter could toggle a dead zone. Offer visual confirmation of cancellation and optional haptics. **Check** repeated slow sweeps across the boundary, cancellation after a large drag, and release directly on the boundary, with vibration on and off. Verify one outcome per release.
4. **Tune reach and obstruction on devices.** Place frequently combined actions where they can be used together. Account for orientation, hand preference, and system-reserved edge regions. Move the preview away from the finger when precise placement is necessary. **Check** small and large phones and a tablet under normal grips. Complete the highest-frequency action sequence without changing grip and record target occlusion, mistaken presses, and reach failures.
