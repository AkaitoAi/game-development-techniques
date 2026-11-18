# UI comparison layouts and stable baselines

## Problem and design reasoning

Players compare options across panels, tooltips, settings, upgrades, and results screens. If labels move, baselines change silently, or one panel hides the other, the interface makes them remember information instead of inspecting it. This guide concerns comparison presentation and interaction, not inventory data ownership or item-scoring formulas.

Blizzard's *Diablo III* retrospective explains that reviewing loot interrupted console couch co-op. Comparison tooltips and Quick Equip helped preserve pace while supporting players' own decisions. The portable lesson is to reduce the cost of checking an option without hiding the information needed to reject a summary. [Blizzard's comparison retrospective](https://news.blizzard.com/en-gb/article/20120910/diablo-then-now-item-comparison-evolution)

David Candland's GDC 2016 talk, *Tenacious Design and The Interface of Destiny*, describes the tension between approachability and depth for experienced players. Its topics include the free cursor and timed interactions. That source supports the interaction-design problem, while the layout and testing recipe below is a recommendation rather than a reconstruction of Bungie's implementation. [Official Destiny session](https://www.gdcvault.com/play/1023107/Tenacious-Design-and-The-Interface)

## Techniques and practical checks

### Declare the baseline

Label the current or pinned choice and the candidate explicitly. State whether a value is absolute or a difference. Keep the baseline fixed while browsing until the player deliberately changes it. If simulation context affects a preview, show that context alongside the result.

**Check** Browse several candidates, pin another baseline, cancel, and reopen. Ask players which option each column represents and what a displayed difference measures.

### Align comparable information

Use shared row keys and stable ordering. Represent unavailable values explicitly rather than collapsing a row and shifting everything beneath it. Align units and precision. Reserve space for long labels and distinguish an absent property from a numeric zero.

**Check** Compare options with missing fields, long localized labels, large values, and opposite desirability rules. Corresponding rows must remain identifiable without relying on color.

### Preserve focus and visibility

Choose whether panels scroll together or independently and signal that behavior. Keep the selected candidate visible when opening details. Support pointer, keyboard, controller, and touch transitions without closing the comparison unexpectedly. Place popovers where they do not cover their evidence.

**Check** Reach the final row with each supported input method, change candidates, then return. Verify focus, scroll position, baseline identity, and unobscured action controls.

### Reveal detail progressively

Start with the few attributes needed for the current decision and offer full details without losing comparison context. Avoid labeling an option universally better when tradeoffs remain. Measure decision accuracy as well as speed, since a faster misleading choice is a failed layout.

**Check** Give players tradeoff decisions with known task goals. Record correct choices, time, detail openings, and accidental confirmations, including at small display sizes and enlarged text.
