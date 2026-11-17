# Procedural animation

## Problem and design reasoning

**Problem.** An authored clip has the right style but a character's hand misses a moving target or its feet slide on uneven ground. Look for errors that vary with runtime conditions.

**Design reasoning.** Rosen demonstrates starting from a small authored pose set and adapting it at runtime. An inferred rule is to store poses for recognizable intent and solve only the contacts or reactions that need current scene data. See the shipped examples below for sources.

Add procedural motion when an authored pose needs to adapt to changing contacts, forces, or targets. Assign each correction an owner and define when it yields to the authored motion.

## Shipped examples

**Black Shades (2002) and Overgrowth (2017).** Wolfire developer David Rosen used examples from both games to demonstrate interactive animation built from a small number of authored poses and procedural adjustments. This approach lets motion respond to runtime context while retaining authored poses as a visual foundation. [David Rosen, GDC 2014 Animation Bootcamp](https://www.gdcvault.com/play/1020583/An)

**Ghost of Tsushima (2020).** Sucker Punch combined procedural gusts and multiple responsive elements to convey wind across the environment. That shared environmental input illustrates how separate moving elements can appear to inhabit the same space. [Sucker Punch visual effects breakdown](https://blog.playstation.com/2021/01/12/how-stunning-visual-effects-bring-ghost-of-tsushima-to-life/) · [Jasmin Patry, SIGGRAPH 2021 Advances in Real-Time Rendering](https://advances.realtimerendering.com/s2021/jpatry_advances2021/index.html)

## Techniques and practical checks

### Correct contacts relative to the support

Mark the planted phase of each limb. While planted, keep a contact target relative to the supporting surface so it follows moving platforms. Solve toward that target within joint and pelvis limits. Release the constraint gradually at liftoff or when the target exceeds reach. Keep gameplay collision authoritative.

**Check** uphill and downhill steps, edge contacts, rotating supports, and a platform that disappears. Measure plant drift in support space and inspect knee direction and pelvis displacement at transfer.

### Blend aim and reach within anatomical limits

Convert the target direction into the rig's local frame, limit it to supported angles, and distribute correction across the intended joints. For a reach, clamp extension and provide a preferred bend direction. Fade corrections through state transitions that require full-body authored motion.

**Check** targets near the center, beyond each angle limit, directly behind the character, and across a moving camera cut. Track endpoint error and the largest per-frame joint turn. Confirm the body retains intentional animation and that target loss restores the expected pose.

### Add secondary motion from shared signals

Drive appendage, cloth-like, or vegetation motion from acceleration or a spatial wind field. Bound spring displacement and use a stable time integration step. Add small per-instance phase offsets when repeated assets move too uniformly. Reset cached velocity on teleports or pooled-object activation.

**Check** at low and high frame rates using the same recorded input and a deliberately long frame. Inspect settling time, axis flips, apparent wind direction, and motion immediately after reactivation.

### Budget corrections by visibility and importance

Keep gameplay-significant contact or aiming data up to date when reducing cosmetic pose work. Use analytic or lower-rate updates for distant instances. On return to full detail, initialize procedural history from the current pose and blend in corrections.

**Check** across distance thresholds with repeated camera crossings. Time the animation pass under a full crowd load and inspect for discontinuities, stale contact targets, and changed gameplay outcomes.
