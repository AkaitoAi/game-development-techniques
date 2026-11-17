# Inventory and crafting

## Problem and design reasoning

**Problem.** A recipe preview promises an output the inventory cannot receive, or transfers silently combine items that differ in quality or freshness.

**Design reasoning.** Minecraft offers two ways to enter the same recipe requirements, while Factorio documents attributes carried by products. Together they suggest keeping recipe resolution authoritative and giving each interface a faithful view of it.

## Shipped examples

**Minecraft (2011).** Mojang's guide for the shipped game distinguishes the inventory crafting grid from the larger crafting table grid. The recipe book offers an alternate input path to arranging materials manually. Both paths express the same recipe requirements. [Mojang crafting guide](https://www.minecraft.net/en-us/article/how-craft)

**Factorio: Space Age (2024).** Wube's design notes describe quality modules that give machine crafting a chance to produce higher-quality items. Its shipped prototype documentation exposes product quality, amount, and freshness rules. These are examples of item attributes that a production system needs to preserve through crafting and storage. [Wube design notes on quality](https://www.factorio.com/blog/post/fff-375) · [Factorio item product prototype](https://lua-api.factorio.com/latest/types/ItemProductPrototype.html)

Renee Gittins's GDC Independent Games Summit material shows a crafting interface with inventory categories and ingredient slots. Treat that layout as an authoring example and validate the interface against the actual decisions your game asks players to make. [GDC 2025 speaker slides](https://media.gdcvault.com/gdc2025/Slides/Gittins_Renee_You%2BAre%2BNot.pdf)

## Techniques and practical checks

### Define item identity and stack compatibility

Separate the stable item definition from each instance's mutable properties. Give a unique item its own identity. Define which fields must agree before units can stack, including durability, quality, and ownership where they matter. Express slot count, stack limits, and weight as explicit container rules.

**Check** split and merge operations at empty, nearly full, and full capacity. Try merging items with different attributes. Confirm that unit counts and instance identities survive transfer, sorting, and save reload.

### Apply a recipe as one operation

Resolve eligible ingredient sources, output counts, destination capacity, and recipe prerequisites against the same snapshot. Commit inputs and outputs together or leave the prior state intact. For a timed craft, define whether inputs are reserved or consumed at start and what cancellation returns.

**Check** craft into a full output, cancel halfway, and submit overlapping actions that need the same input. Verify exact ingredient consumption, one output per completed craft, and predictable recovery when a destination disappears.

### Give players a reason for blocked recipes

Show availability, required station, missing quantities, and expected output from the same rules used for confirmation. Distinguish discovered recipes from immediately craftable recipes and update both when inventories or station access change.

**Check** ask players to craft an item with one missing ingredient and then change the item in a second container. Confirm the display agrees with the actual result and that both pointer and controller users can identify the next step.

### Validate the recipe graph and progression

Build a graph from input types to outputs. Mark starter items and sources outside crafting. Evaluate reachability with the relevant unlocks and stations. Review intentional loops such as recycling for net generation and storage bottlenecks.

**Check** run a cold-start fixture for every mandatory craft. Inject a missing starter source, a locked prerequisite, and a self-supplying cycle. Confirm validation catches unreachable content without rejecting a cycle with a valid external input.
