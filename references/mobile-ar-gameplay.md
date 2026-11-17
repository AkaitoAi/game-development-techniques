# Mobile AR gameplay

## Problem and design reasoning

A stable camera feature must recover from dim light, brief tracking loss, permission denial, and unsupported hardware. Niantic's talk explains using AR to bring Pokémon to life in players' surroundings. Its GDC 2025 panel discussed spatial features across shipped games such as *Pokémon GO* and *Peridot*. An implementation inference is to preserve the action while the device rebuilds its spatial estimate. [Niantic, AR Game Design 101](https://gdcvault.com/play/1026885/AR-Game-Design) · [Niantic Spatial for Games, GDC 2025](https://www.nianticspatial.com/en/blog/gdc-2025-niantic-spatial-computing-ar-recap)

## Techniques and practical checks

### Gate placement by confidence

Preview an eligible surface and commit after the player confirms placement.

**Check** Try a blank wall, reflective surface, and textured floor. Verify placement waits for a valid result and shows a usable alternative.

### Separate anchor from gameplay state

Hold an interaction while tracking degrades and reacquire or offer a reset without repeating a completed action.

**Check** Cover and uncover the camera during placement and after scoring. Check object position and reward counts.

### Give camera access a clear purpose

Explain which interaction uses the camera and offer an alternative if camera features are optional.

**Check** Test denied, revoked, and unsupported camera access. Verify the player can leave the feature and keep earned progress.

### Budget camera work on target devices

Measure tracking, compositing, and frame time together over a representative session.

**Check** Run sustained device tests with fast turns and low light. Record tracking loss, dropped frames, and recovery time.
