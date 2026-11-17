# Climbing and mantling

## Problem and design reasoning

A jump can place the character near a ledge while the controller, animation, and level geometry disagree about whether the character should catch it. Routes become difficult to learn when similar-looking surfaces have different rules. Ubisoft explained that requiring physical handholds in *Assassin's Creed Shadows* gave level designers more control over vertical routes and reinforced the protagonists' different movement capabilities. An inference for other projects is to make climbability more authored when route choice matters, and to prioritize consistent clearance rules when climbing is primarily a way to maintain movement flow. [Ubisoft Québec, Assassin's Creed Shadows parkour overview](https://quebec.ubisoft.com/en/assassins-creed-shadows-parkour-system-overview/)

## Shipped examples

- **Assassin's Creed III (2012).** Ubisoft developers discussed improving the series' climbing and running as part of their parkour work. Use their GDC postmortem as a case study in treating locomotion and the traversable environment as parts of the same feature. [Richard Dumas and Aleissia Laidacker, AI Postmortems, GDC 2013](https://www.gdcvault.com/play/1019072/AI-Postmortems-Assassin-s-Creed)
- **Assassin's Creed Shadows (2025).** Both protagonists can climb using physical handholds, and Naoe's grappling hook extends access to some ledges. Ubisoft also describes holding the character at dangerous drops until the player commits to the descent or climbs back up. [Ubisoft Québec, parkour system overview](https://quebec.ubisoft.com/en/assassins-creed-shadows-parkour-system-overview/)

## Techniques and practical checks

### Define eligibility and clearance

Use authored handles, geometry queries, or a combination to form candidates. Check approach direction, reach, material or surface metadata, and the body volume during and after the move. A mantle should validate the space occupied by the movement arc as well as the final standing or crouching pose.

**Check** ledges just below and above the reach limit, a low ceiling, a corner, and a wall without a standable top. Log which rule accepts or rejects each candidate. Repeat for every supported character size.

### Choose a stable target and show intent

Rank valid candidates by input direction, reach, and alignment. Keep the existing target while it remains valid unless a new candidate beats it by a meaningful margin. Use consistent visual language for surfaces intended to be climbable, and expose enough feedback for a player to understand which target the input will choose.

**Check** two nearby ledges at an intersection, diagonally held movement, and a rotating camera. Compare the highlighted or anticipated destination with the destination actually selected on repeated runs.

### Coordinate animation and collision

Define whether the controller drives displacement or consumes motion from animation during the transition. Align the animation to a contact anchor within a bounded correction, and continue collision checks while moving. Specify interruption behavior for blocked or invalidated destinations.

**Check** mantle onto a moving or rotating platform, then introduce an obstacle and remove a ledge mid-move in a test scene. Verify that hands and feet track the intended anchor, the body stays clear of blocking geometry, and recovery returns control.

### Validate routes with each movement ability

Treat climb, hang, mantle, drop, jump, and special movement as transitions with stated requirements and safe exits. Validate important routes against the intended abilities, character size, and any loadout restrictions. Teach dangerous descents with visible setup and a chance to reverse the decision where appropriate.

**Check** attempt the route ascending and descending with every supported ability set. Include held input at its boundaries, an interrupted climb, and a reload at a route checkpoint. Verify that mandatory paths are reachable and optional shortcuts remain intentional.
