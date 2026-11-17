# Maps and navigation aids

## Problem and design reasoning

**Problem.** Players get lost because a cue is missing, or they follow the interface so closely that they stop looking at the world.

**Design reasoning.** Sucker Punch describes using wind and environmental cues to keep attention on the world. Interpret that as matching the cue to the decision at hand, with a map or an explicit marker available where players need more context.

## Shipped examples

**Doom (1993).** The released Linux source shows the automap drawing world lines and a player arrow into a separately controlled viewport. Players can pan, zoom, toggle follow mode, and mark positions. Map line display checks world flags, which separates the geometry available to the engine from the information available to the player. [id Software automap source](https://github.com/id-Software/DOOM/blob/master/linuxdoom-1.10/am_map.c)

**The Legend of Zelda: Phantom Hourglass (2007).** Nintendo's manual describes displaying the map on the touch screen and writing on it. The map also serves as a workspace where players can keep their own notes. [Nintendo game manual](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/nintendo_ds_21/Manual_NintendoDS_TheLegendOfZeldaPhantomHourglass_EN.pdf)

**Ghost of Tsushima (2020).** Sucker Punch describes how wind and environmental cues keep players looking at the world while navigating. Its programming presentation explains how multiple visible systems respond to changes in wind direction. A directional cue can guide attention while the world remains the primary navigation surface. [Sucker Punch exploration talk, GDC 2021](https://gdcvault.com/play/1027399/Exploration-in-Ghost-of-Tsushima) · [Guiding Wind simulation talk, GDC 2021](https://www.gdcvault.com/play/1027350/Blowing-from-the-West-Simulating)

## Techniques and practical checks

### Model what the player knows

Store discovery and objective reveal state separately from geometry and routing data. Decide when areas, shortcuts, objective locations, and player pins appear, remain after travel, or become hidden by game rules. Generate map symbols from that knowledge state.

**Check** approach the same landmark from multiple entrances, save and reload, then revisit after a quest change. Inspect map, compass, and objective panel for consistent reveal state and concealed locations.

### Handle floors and spaces explicitly

Map each world position to a region, floor, and two-dimensional map coordinate. Use connectors such as stairs or lifts to show how floors relate. Preserve a clear heading reference when switching between north-up and player-up modes.

**Check** stand above and below an objective, traverse overlapping floor plans, and zoom while rotating. Verify the marker distinguishes vertical separation and the displayed connector reaches the target floor.

### Distinguish bearing from a traversable route

Show a bearing when players are expected to find their own path. When promising turn-by-turn guidance, build the route from traversable links and filter them by current abilities, locks, and the active mode of travel. Recompute on meaningful route changes and mark unreachable destinations explicitly.

**Check** place a target across a locked gate, under a bridge, and behind a one-way drop. Unlock or close a connector while navigation is active. Confirm the aid communicates a usable path or clearly signals that none is available.

### Keep cues legible under competing demands

Rank markers by quest relevance and the player's selected target. Cluster nearby markers at low zoom while keeping the selected marker identifiable. Provide a second cue where the primary cue relies on sound, color, or subtle motion. Give players clear controls for placing and removing their own markers.

**Check** navigate with the busiest map layer active, then repeat with sound muted and under color vision simulation. Ask first-time players to identify the destination and make the next correct turn without needing to reopen the map.
