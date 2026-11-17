# Strategy and management simulation

Management simulation asks players to intervene in a system that keeps changing. They need ways to see what is happening, forecast what a decision will do, and adjust when the result differs from their expectation.

## How shipped simulations make decisions legible

- **SimCity (1989)** links zoning, electricity, taxation, and growth. The original manual explains that development stops in unpowered zones and that high taxes can suppress growth. A budget decision can change both municipal income and the willingness of residents and businesses to locate in the city. [Maxis's original manual, archived scan](https://drwjf.github.io/ctm/references/SimCity_1989_Manual_EN.pdf)
- **Factorio: Space Age (2024)** adds a science output graph to the production interface. It measures final science progress, which the count of packs consumed alone cannot show when lab productivity changes. The example illustrates why the quantity players need to manage may differ from the quantity easiest to count. [Wube Software's statistics design notes](https://direct.factorio.com/blog/post/fff-408)
- **Frostpunk 2 (2024)** delegates lawmaking to a council whose delegates vote. Negotiations and the divided interests of factions affect what a player can enact even when the material balance looks favorable. The sequel shifts the design emphasis from emergency survival toward managing the competing visions of a surviving society. [11 bit studios' gameplay overview](https://11bitstudios.com/frostpunk-2-on-the-first-gameplay-trailer/) · [Co-director Kuba Stokalski's launch essay](https://news.xbox.com/en-us/2024/09/20/why-frostpunk-2-is-a-true-and-creatively-necessary-sequel/) · [Stokalski's Taipei Game Developers Forum session](https://2025.tgdf.tw/en/speakers/19)

These examples span three levels of explanation. SimCity's manual describes citywide dependencies. Factorio's statistics expose measured flows during play. Frostpunk 2 makes another actor's approval a requirement the player must account for when planning an intervention.

## Techniques

### Write the stock-and-flow model first

For each resource, list its sources, sinks, capacity, unit, and accounting interval. Prototype a district where stock changes by inflow minus outflow, with rejected overflow and unmet demand tracked separately. Give the player one lever that changes flow and another that changes capacity. **Check.** Calculate expected totals for an empty, stable, and saturated district. Run the same sequence at different simulation speeds and compare results after equal simulated time.

### Show a bottleneck in context

Expose stock, recent net change, and the biggest contributors where players act on shortages. Lead from a warning to its affected district or production chain. Keep recent trends available so players can distinguish transient spikes from persistent deficits. **Check.** Plant a supply shortage, blocked transport, and rising demand in otherwise similar scenarios. See whether players identify the correct cause and take an action that resolves it.

### Make gains compete with other goals

Put two desired outcomes in conflict by sharing a limited budget, workforce, or political support. Apply consequences over an understandable time window, and show what has been committed. **Check.** Run several scenarios with growth, scarcity, and disrupted supply. Try each policy on the same starting states and verify that at least two choices have a plausible use under different constraints.

### Preserve causality as the simulation grows

Use a documented update order for generation, transport, consumption, and deferred effects. Capture a seed and intervention log so the same scenario can be replayed for balancing. If events from independent systems can compound across thresholds, expose the first material cause in the interface. **Check.** Save just before a shortage and compare the next interval before and after loading. Repeat with two causes firing on the same tick and confirm totals, alerts, and visible outcomes agree.

## Problem and design reasoning

**Problem.** A player can see a falling output number but cannot locate the supply or capacity constraint behind it. Compare available signals with the quantities players can act on.

**Design reasoning.** Wube describes adding a science-output graph because pack consumption cannot capture changing lab productivity. The inferred design principle is to expose the consequence the player is trying to manage and retain intermediate counters for diagnosis. See the shipped examples below for sources.
