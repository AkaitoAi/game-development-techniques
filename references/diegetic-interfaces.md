# Diegetic interfaces

## Problem and design reasoning

Players sometimes need information while watching the world and responding to changes in it. Moving a cue into a prop, character, or fictional instrument can keep that information near its subject, but perspective, movement, and occlusion can make it unreadable. In his GDC talk, *Dead Space* franchise lead UI designer Dino Ignacio describes early attempts that led the team toward an interface presented within the fiction. An implementation inference is to decide which information belongs to a particular object and which needs a guaranteed presentation when the object leaves the view. [Dino Ignacio, GDC 2013, Crafting Destruction](https://www.gdcvault.com/play/1017723/Crafting-Destruction-The-Evolution-of)

## Shipped-game examples

- **Dead Space (2008)** made its interface part of the setting. Ignacio's retrospective examines how the interface evolved toward this presentation. The remake retains a distinct log interface with controls for closing it, an example of how an in-world presentation still needs explicit input behavior. [Dino Ignacio, GDC 2013](https://www.gdcvault.com/play/1017723/Crafting-Destruction-The-Evolution-of) · [EA accessibility controls for the remake](https://www.ea.com/able/resources/dead-space/dead-space/pc/gameplay)
- **Metroid Prime (2002)** represents information gathering as a function of Samus's suit. The Scan Visor signals scannable entities, downloads information during an explicit interaction, and makes some findings available later in the Log Book. Its Thermal Visor exposes another set of clues. [Nintendo's original manual, Behind the Visors](https://csassets.nintendo.com/noaext/image/private/t_KA_PDF/GCN_Metroid_Prime?_a=DATC1RAAZAA0)

## Techniques and practical checks

### Attach information to the entity it describes

Bind the display to the same gameplay state that drives the underlying rule. Define which cameras can show it and how its pose follows its host. Use a stable angle or alternative cue when the camera cannot reliably see the host.

**Check** the same state change in normal play, while the host is rotated, and while it is partially hidden. Confirm players can identify the affected object and the new state quickly enough to act.

### Give in-world tools clear interaction states

For tools like scanners and terminals, show whether an object is eligible, selected, active, complete, or out of range. Define what happens when the player breaks contact or changes modes. Keep interaction state separate from the animation of the visible device.

**Check** releasing the interaction, changing targets, and leaving range just before completion. Verify the correct item is recorded once and progress feedback matches the actual state.

### Protect critical cues from presentation failures

Set a minimum readable size, contrast target, or alternative channel for information with immediate consequences. Use color plus shape, text, or sound where appropriate. Preserve an accessible alternative when a spatial cue is hard to read.

**Check** dark and bright scenes, fast motion, a narrow viewport, reduced effects, and muted audio. Ask players to identify the cue and the action it calls for. Verify the alternative updates at the same time as the world display.

### Decide who owns attention and input

Specify whether inspecting an instrument pauses the simulation or keeps the world active. Define action routing while it is open, how the player can dismiss it, and what interrupts the display. Keep the focused label or target predictable when the player returns to play.

**Check** open the display during movement, interrupt it with an authored event, and dismiss it during a mode change. Confirm the player can recover control and that closing the display has the intended effect when inputs are shared.
