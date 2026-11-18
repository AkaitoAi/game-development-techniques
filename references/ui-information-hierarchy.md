# UI information hierarchy

## Problem and design reasoning

A HUD can expose every relevant value while players still miss the one that determines their next action. Housemarque's *Returnal* UI director describes placing immediate survival information near the reticle, peripheral information at the HUD edges, and strong multisensory feedback around low health. The team iterated timing, color coding, and hierarchy around bullet-hell combat. This is a documented attention allocation decision. [Johannes Koski, Returnal UX design](https://blog.playstation.com/2021/05/11/unpacking-returnals-ux-design-gameplay-first-ui-retro-futuristic-tech-and-accessibility/)

Dino Ignacio's GDC presentation traces how the *Dead Space* team arrived at its shipped diegetic interface. It provides a second case where presentation belongs to the game's fiction while remaining part of the player's information system. The workflow below is a portable recommendation. It prioritizes decisions by urgency and consequence, then tests whether the chosen presentation communicates them under load. [Dino Ignacio, Crafting Destruction: The Evolution of the Dead Space User Interface, GDC 2013](https://www.gdcvault.com/play/1017723/Crafting-Destruction-The-Evolution-of)

## Techniques and practical checks

### Rank facts by the decision they support

List what players must notice now, what they need for the next decision, and what they can inspect between actions. Identify the consequence of missing each fact. Assign persistent screen space and strong accents to the information that justifies them. Keep ordering stable enough that players learn where to look.

**Check** ask players to complete a representative encounter and immediately identify health danger, available action, and next objective. Compare missed facts and response delay with the intended priority, including a busy scene.

### Encode importance across several properties

Use placement, grouping, size, contrast, motion, and redundant sensory cues as a coordinated vocabulary. Reserve a distinct treatment for urgent changes. Avoid allowing every new notification to borrow the highest-priority treatment, and preserve meaningful differences when color alone becomes unavailable.

**Check** combine a reward notification, objective update, and low-health event. Ask players to identify the urgent condition with normal settings, reduced motion, and audio muted. Inspect whether overlays conceal the critical state.

### Keep progressive disclosure tied to context

Show short, decision-relevant information in play and make deeper explanations available through an intentional action. Preserve the item, target, or selection that caused the detail view to open. When an action is unavailable, expose the reason where the player attempts it.

**Check** move from a compact HUD cue to its detailed explanation and back using every supported input method. Confirm focus returns to a useful place and that long localized text remains readable.

### Validate in motion and at release scale

Evaluate the interface with gameplay camera motion, actual display size, shipping effects, and representative text. Record which cue was visible when an action became necessary. Distinguish a failure to notice the cue from misunderstanding it or failing the action.

**Check** capture a repeatable route on the smallest supported display and a common desktop or television setup where relevant. Have unbriefed players describe cues and predict outcomes. Log recognition, interpretation, and execution separately.

See [User interface](user-interface.md) for implementation choices and [Color and visual readability](color-and-visual-readability.md) for contrast and cue encoding.
