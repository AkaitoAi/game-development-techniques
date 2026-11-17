# Survival and base building

Survival systems give players a reason to leave safety and a deadline or cost for returning. Building lets the player change that equation by creating shelter, staging supplies, or extending the reach of later expeditions.

## How shipped games connect risk and construction

- **Minecraft (2011)** turns sunset into an early deadline. Gathering and crafting make a crude shelter feasible before night falls, and the shelter gives the player a reason to learn the construction system immediately. [Mojang's survival guide](https://www.minecraft.net/en-us/article/how-minecraft) · [Xbox's release retrospective](https://news.xbox.com/en-us/2024/05/17/minecraft-15-years-most-important-moments/)
- **Subnautica (2018)** connects exploration to where the player can resupply and find air. Modular habitats allow an outpost to gain corridors, hatches, and reinforcements, while the Cyclops supports long trips as a mobile home base. Director Charlie Cleveland describes building the game around exploration, discovery, and the unknown. [Unknown Worlds' habitat update](https://unknownworlds.com/en/news/habitat-update-released) · [Cyclops introduction](https://unknownworlds.com/en/news/cyclops-submarine-released) · [Cleveland's GDC 2019 talk](https://gdcvault.com/play/1025745/The-Design-of-Subnautica)
- **RimWorld (2018)** scales the problem to a settlement. The AI storyteller selects events such as storms, raids, and visiting traders. Different storytellers change how pressure builds, so the settlement layout and stockpile become preparation for a range of possible setbacks. [Ludeon Studios' game overview](https://rimworldgame.com/) · [Tynan Sylvester's GDC 2017 talk](https://www.youtube.com/watch?v=VdqhHKjepiE)

These examples move from a shelter that answers a predictable environmental cycle, to outposts that make previously impractical trips possible, to a settlement whose infrastructure has to handle interacting and uncertain events.

## Techniques

### Prototype one expedition and return

Give the player a conspicuous resource or landmark beyond shelter. Choose one pressure such as daylight or air and make the return cost legible before departure. Use the haul to expand a subsequent expedition or improve the fallback route. **Check.** Run the same trip with a direct route, one detour, and one retreat. Ask players at each fork how much time remains and where they would go if forced to turn back.

### Make the first shelter achievable

Define the smallest room or structure that provides an immediate benefit. Give it several plausible sites and an obtainable bill of materials near the starting area. Allow the player to repair or extend that starter structure. **Check.** Try poor but supported starts, a partially consumed inventory, and an interrupted build. Verify that first-time players can identify a viable shelter and finish it before the first ordinary threat.

### Preview placement and its consequences

Show the footprint, valid connections, obstructions, and any load or power change that matters. Validate again when placement is committed and deduct resources only after it succeeds. **Check.** Attempt a placement at an occupied boundary and after dismantling its support or consuming required materials. Verify that the preview explains the rejection and that failed placement leaves inventory and existing structures consistent.

### Give setbacks a recovery route

For a threat such as a storm or raid, state its warning signs and what preparation changes. Prototype a damaged-but-recoverable base using either stored supplies, a substitute shelter, or a nearby resupply route. **Check.** Break a key dependency during an ordinary run and replay from that state. Verify both a recoverable path for a reasonably prepared player and a clear account of why the base failed when recovery is impossible.

## Problem and design reasoning

**Problem.** A survival loop oscillates between a trivial routine and setbacks that leave no recovery, or bases become decorations detached from exploration. Observe whether new construction changes what trips players can take.

**Design reasoning.** Cleveland describes centering Subnautica on exploration and discovery. An inference from its habitats and mobile base is that shelter supports that priority when it extends the player's reach into unfamiliar areas. See the shipped examples below for sources.
