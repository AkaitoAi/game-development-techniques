# UI icon system design

## Problem and design reasoning

An icon can remain visible while its meaning is ambiguous. Similar silhouettes, inconsistent state badges, or artwork that only reads at authoring size can make a quick action require repeated tooltip inspection. Santa Monica's shipped *God of War Ragnarök* supports separate icon scaling and pairs interaction prompts with optional audio cues. Mila Pavlin describes the broader UI redesign around flexibility and readability. These are documented presentation choices. [Mila Pavlin, God of War Ragnarök accessibility features](https://blog.playstation.com/2022/05/19/god-of-war-ragnarok-accessibility-features-revealed/)

Dino Ignacio's *Dead Space* presentation documents the interface concepts that evolved into the shipped diegetic system. It provides evidence that a visual language must work in its game's context. The symbol grammar and recognition tests below are portable recommendations. They extend that concern to the meanings players distinguish between individual icons. [Dino Ignacio, Crafting Destruction: The Evolution of the Dead Space User Interface, GDC 2013](https://www.gdcvault.com/play/1017723/Crafting-Destruction-The-Evolution-of)

## Techniques and practical checks

### Define the vocabulary before drawing variants

List each concept the player must distinguish, the action or decision it supports, and its likely confusions. Give one stable symbol to one meaning within a context. Specify how a symbol changes for unavailable, selected, equipped, or pending states. Keep item identity and action status as separately readable information where both matter.

**Check** present icons at their actual displayed size beside the nearest confusing alternatives. Ask unfamiliar players to match them to a short set of meanings and explain their choice. Record the confusion pairs and redesign the ambiguous silhouettes before expanding the set.

### Establish a consistent visual grammar

Set an optical size, stroke weight, silhouette style, padding, and badge placement. Use shape as well as color for information required to act. Author simplified versions when a detailed illustration fails at a smaller size. Give source assets stable semantic IDs so an art replacement preserves the intended meaning.

**Check** compare the complete set on the darkest, brightest, and busiest supported backgrounds. Repeat at minimum and maximum UI scale and with color-reduced captures. Confirm states remain distinguishable without zooming into a screenshot.

### Keep icons and their explanations coherent

Bind the icon, localized label, tooltip, accessibility name, and relevant control prompt to the same meaning. A platform button glyph identifies an input, while the action icon identifies what it does. Refresh both when the context or binding changes, and provide a discoverable label when an unfamiliar symbol needs explanation.

**Check** change input device, rebind an action, switch language, and open a controller-focused tooltip. Verify that the displayed symbol, spoken or textual name, and resulting action agree in each state.

### Measure recognition during the task

Test the icon within the encounter, menu, or inventory decision where it is used. Measure wrong choices and time to locate a known action alongside subjective preference. Preserve learned meanings when updating art, or provide a transition that teaches the changed vocabulary.

**Check** run the same small set of decisions with the current and candidate icon sets using comparable player groups. Include a returning player session. Inspect confusion and lookup errors rather than treating visual preference alone as evidence of recognition.

See [UI information hierarchy](ui-information-hierarchy.md) for attention priority and [UI feedback and confirmation](ui-feedback-and-confirmation.md) for communicating action outcomes.
