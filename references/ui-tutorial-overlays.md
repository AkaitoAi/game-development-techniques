# UI tutorial overlays

## Problem and design reasoning

A tutorial overlay can explain an action while covering the object the player needs to inspect or claiming the input needed to perform it. A prompt that remains after the player succeeds can teach the wrong state. In *Returnal*, Housemarque describes placing critical information near the reticle and iterating its hierarchy, motion, and accessibility as part of the gameplay experience. That supports evaluating teaching UI at the actual moment of play. [Johannes Koski, Returnal UX design](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

Valve's *Portal* postmortem describes changing a chamber after players struggled and using dialogue when environmental cues were insufficient. The documented lesson is to respond to observed confusion. The contextual eligibility, overlay ownership, and completion rules below are portable recommendations, rather than claims about either studio's internal implementation. This guide concerns the presentation and lifetime of a teaching overlay. Use [Onboarding and tutorials](onboarding-and-tutorials.md) to design the learning sequence. [Kim Swift and Erik Wolpaw, Portal Postmortem, GDC 2008](https://cdn.akamai.steamstatic.com/apps/valve/2008/GDC2008_PortalPostMortem.pdf)

## Techniques and practical checks

### Tie the overlay to a current action opportunity

Define the action being taught, the state in which it is possible, and the observable action that completes the lesson. Reevaluate eligibility before showing a queued prompt. Derive glyphs from current bindings and hide the overlay when its target or context becomes invalid.

**Check** enter the context, complete the action before the overlay appears, then leave and reenter. Verify the prompt never asks for an already completed or currently impossible action. Repeat after remapping and switching input devices.

### Preserve the object and information needed to act

Anchor the explanation close enough to associate with its subject while keeping the target, action feedback, and hazards visible. Test placement with localized text, safe areas, and the shipping camera. Choose a layout fallback when an anchor is off-screen.

**Check** show the overlay on the smallest supported viewport with the longest supported text and large text settings. Ask a new player to identify the target and perform the action. Record missed targets, obscured cues, and reading failures.

### Give modal teaching explicit input ownership

Decide whether play pauses, which controls remain active, and how dismissal works. Prevent a dismissal press from also activating the underlying screen or game action unless that is the intended lesson. Return focus to a valid control after closing.

**Check** dismiss with held, repeated, and rapid inputs, then reopen through the normal help route. Open another modal while the lesson is active. Verify one input produces the intended action and focus returns to a usable location.

### Keep completion and dismissal distinct

Record that an overlay was seen separately from evidence the player performed its action. Permit a useful way to request help again. Persist only the lesson state required by the game's resume policy, and restore the practice setup when needed.

**Check** dismiss without acting, quit during the lesson, and resume after completing it. Verify the relevant help remains available, the action is still reachable, and the lesson can be completed independently in a changed situation.
