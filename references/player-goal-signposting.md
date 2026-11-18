# Player goal signposting

## Problem and design reasoning

A player can know where they are and still misunderstand what the game now expects them to do. This guide concerns the handoff between goals, including optional distractions, completion, and resuming after a break. [Maps and navigation aids](maps-and-navigation-aids.md) covers spatial representation and route guidance.

Sucker Punch's Jason Connell explains that *Ghost of Tsushima* turned its visually prominent wind into a way of directing players toward tracked objectives while keeping their attention in the world. Parker Hamilton's GDC 2021 talk describes exploration tools that encourage discovery and support the samurai fantasy. These sources document the intended attention model. The goal-state and recovery contracts below are portable recommendations. [Sucker Punch developer Q&A](https://blog.playstation.com/2020/05/29/ghost-of-tsushima-your-questions-answered/) · [Parker Hamilton, Exploration in Ghost of Tsushima, GDC 2021](https://gdcvault.com/play/1027399/Exploration-in-Ghost-of-Tsushima)

## Practical techniques

### Give the next action an explicit meaning

For each goal transition, specify what the player should understand, which action advances the goal, and how the game acknowledges that change. Use action-oriented wording and a visible world response where possible. Keep quest state, prompts, and guidance on the same active objective revision.

**Check** start immediately before and after three transitions, such as reaching a destination, finishing an interaction, and discovering a new lead. Ask an unfamiliar player to name the next action before giving input. Compare the answer with the actual completion condition.

### Distinguish discovery from commitment

Make it clear when a new activity is an optional lead, the selected destination, or a required continuation. Let the player return to the previously selected objective after investigating a distraction. Avoid letting every nearby cue compete at the same priority.

**Check** discover an optional activity while following a main objective. Accept it, abandon it, and return. Verify the active goal is identifiable after each choice and that guidance points to the corresponding playable target.

### Retire completed cues coherently

When a goal completes, update its world cue, prompt, journal, and any spoken instruction according to one outcome. If the next goal is delayed, show a meaningful waiting state instead of continuing to invite the completed action. Preserve the state across scene or session changes.

**Check** complete an objective while its guidance is visible, then interrupt with pause, scene load, and save reload. Confirm the completed target stays complete and the next cue appears only when its required state is ready.

### Offer recovery after missed information

Provide a way to re-obtain the current goal when a player misses dialogue or returns after a break. A short objective reminder can name the intended action and relevant context. Use additional guidance in response to observed confusion if the game supports it.

**Check** skip the initial instruction and return to the session after a break. Record time to correctly identify the next action, wrong interactions, and requests for help. Repeat with audio muted and compare comprehension.
