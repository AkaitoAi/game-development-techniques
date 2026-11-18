# Tween systems

## Problem and design reasoning

A tween updates a value over time, but a reusable runtime also needs ownership, interruption, and completion rules. In Celeste's published player code, a one-shot tween animates a carry offset; another drives respawn presentation and is removed when that state ends. This demonstrates tying interpolation to a concrete state lifetime. Blizzard's Hearthstone UI talk documents the production goals behind a cohesive animated interface. It does not establish which tween library Blizzard used. The implementation inference is to give each transition a clear owner and distinguish finishing normally from cancellation. [Noel Berry, published Celeste player code](https://github.com/NoelFB/Celeste/blob/master/Source/Player/Player.cs) · [Derek Sakamoto, Hearthstone: How to Create an Immersive User Interface, GDC 2015](https://www.gdcvault.com/play/1022036/Hearthstone-How-to-Create-an)

## Techniques and practical checks

- **Sample an explicit timeline.** For duration greater than zero, compute normalized progress as `u = clamp((time - startTime) / duration, 0, 1)`, then evaluate easing and interpolate from captured start to target. Define zero-duration behavior and whether the target can change during playback. **Check** the same transition at several frame rates, a frame longer than the duration, and zero duration. Verify its final value is exact and completion occurs once.
- **Give each property a write policy.** Assign exclusive ownership, replacement, queued sequencing, or an explicit blend for concurrent tweens targeting the same property. Capture a replacement's start value from the current displayed state when continuity is intended. **Check** rapidly opening and closing a panel and changing its target halfway through. Verify the resulting position and alpha match the policy without competing writers.
- **Scope callbacks to lifetime.** Cancel an object's active tweens before reusing or destroying it. Give tween handles a generation or equivalent validation so an old callback cannot mutate a recycled target. Separate cancelled and completed outcomes and define which may schedule follow-up work. **Check** destruction just before completion, pooling and reuse, callback-triggered cancellation, and replacement on the completion tick. Confirm no stale callbacks and a bounded callback count.
- **Specify clocks and sequence boundaries.** Choose scaled gameplay time or an independent presentation clock per transition. Define how delayed segments, loops, and chained actions consume elapsed time when a frame crosses several boundaries. **Check** pause, resume, slow motion, and a hitch crossing multiple sequence segments. Confirm the intended endpoint and callback order, including when reduced-motion settings shorten transitions.

The tween.js maintainer guide provides concrete API examples for grouping, stopping, chaining, and callback behavior. Use the target engine's equivalent while preserving the chosen lifecycle contract. [tween.js user guide](https://tweenjs.github.io/tween.js/docs/user_guide.html)

For the clocks those transitions use, see [Game clock domains](game-clock-domains.md).
