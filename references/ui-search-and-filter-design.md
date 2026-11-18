# UI search and filter design

## Problem and design reasoning

A growing collection can make an inventory technically usable but slow to navigate. Search reduces the set by a query, filters constrain attributes, and sorting determines which surviving result appears first. Their interaction needs a visible contract.

Blizzard's shipped *Diablo IV* update added stash search and filters to help players find items faster, and grouped identical extracted powers when sorting. David Candland's *Destiny* GDC talk describes the broader challenge of keeping a deep interface approachable to new players and efficient for experienced players. These are documented product goals. The result-state and focus protocols below are portable recommendations. [Blizzard, Diablo IV patch notes](https://news.blizzard.com/en-gb/article/24092663/diablo-iv-patch-notes-1-0-1-2) · [David Candland, GDC 2016](https://gdcvault.com/play/1023460/Tenacious-Design-and-The-Interface)

## Techniques and practical checks

### Specify query and filter meaning

Decide which fields search matches, whether terms combine, and how filters intersect. Make active constraints and the filtered result count visible. Offer a clear reset.

**Check** use a known fixture with duplicate names and differing attributes. Compare expected IDs for each individual constraint and combined query, including an empty result.

### Distinguish no results from unavailable data

Show whether the collection is empty, a query matched nothing, or data is still loading. Preserve the current query on retry and explain which constraint can be removed.

**Check** search a populated collection, clear it, and interrupt its load. Verify each state has the correct message and useful next action.

### Preserve selection by identity

Keep focus on the same item when it remains in the result set. When it disappears, choose a deterministic neighbor or results container. Keep text-entry focus separate from item-action focus.

**Check** filter out the selected row, sort, remove an item, and switch input devices. Confirm the visible selection and action target agree after each change.

### Validate labels and real collection sizes

Use player-recognizable names and expose enough information to explain why an item matches. Test localized text and the largest supported collection with keyboard, controller, and touch where applicable.

**Check** ask players to find three specified items without coaching. Record time, erroneous selections, query changes, and performance. Compare against the previous interface on the same fixture.

See [UI list virtualization](ui-list-virtualization.md) and [Inventory UI and item comparison](inventory-ui-and-item-comparison.md).
