# Level blockout and metrics

## Problem and design reasoning

**Problem.** A space that reads clearly in a plan can fail at player scale. The camera clips through entrances, jumps require an unintended run-up, or an objective becomes visible too late. If geometry is already detailed, testing a different layout also means reworking art, collision, and related content.

**Documented reasoning.** First Contact Entertainment describes using a playable blockout of the *Firewall Zero Hour* Hangar map to iterate quickly before committing to the layout and art. A transferable inference is to invest first in the fixtures that answer a design question, such as a doorway that tests clearance or a ramp that tests whether players notice and can reach a route. [Damoun Shabestari, First Contact Entertainment](https://blog.playstation.com/?p=213009)

## Shipped examples

- **Uncharted 3: Drake's Deception.** Naughty Dog produced rudimentary 3D versions of candidate Dry Docks layouts. Lead designer Robert Cogburn took one promising single-player blockmesh and edited it for competitive multiplayer. A cheap representation made the layout available for a different purpose before its original context was locked in. [Robert Cogburn, Naughty Dog](https://blog.playstation.com/2013/10/29/uncharted-3-celebrates-its-two-year-anniversary-with-a-new-map/)
- **Titanfall 2.** David Shaver's GDC workshop shows a ramp that launches the player toward a lit opening. The geometry supplies the action while a simple light cue makes the destination apparent. His workshop puts playable layout and playtesting early in the level workflow. [David Shaver, Invisible Intuition, GDC 2018 slides](https://media.gdcvault.com/gdc2018/presentations/DShaver_Invisible_Intuition_GDC2018.pdf)

## Techniques and practical checks

### Derive a playable metrics kit

Record the actual collision envelope, camera clearance, step and slope limits, and useful jump distances under the game's supported locomotion modes. Build adjustable fixtures for doorways, heights, corners, slopes, ledges, and gaps. Derive a comfortable design value and a boundary value by testing the controller, including different approach speeds and angles.

**Check** each fixture with all bodies and camera modes that can enter the space. Confirm that expected dimensions pass consistently, marginal sizes behave as documented, and there is enough clearance for any required turn or landing.

### Prototype the route with comparable variants

Use inexpensive solids with representative collision and enough lighting or contrast to expose the intended route. Clone a short section and change one geometric variable, such as opening width, sightline distance, or ramp angle. Keep the player start and objective fixed so observations can be attributed to the change.

**Check** both versions with fresh players. Mark where they first recognize the route, where they commit to movement, and how often they hesitate or miss. Counterbalance the order when the same people test both versions, since knowing the layout changes the result.

### Measure travel and decisions in play

Mark key points in the level and log time from entry to cue recognition, action, and arrival. Test at least a clean run and a plausible recovery path. In maps with competing starts, measure reachable routes and arrival times for each side using the movement options available in that mode.

**Check** rapid and cautious runs, alternate entrances, and a wrong turn. Compare route timings against the desired encounter rhythm. Inspect outliers in recordings to distinguish confusion, deliberate exploration, and geometric obstruction.

### Preserve the approved envelopes through art handoff

Record clearance volumes, traversable faces, sightlines, and required landing areas in a shared handoff asset or checklist. Keep a known-good blockout for comparison. Re-run navigation, collision, and camera fixtures when geometry or collision proxies change.

**Check** the same input or route before and after the art pass. Look for narrowed doorways, snagging decoration, occluded cues, added climbing surfaces, and changes in traversal timing.
