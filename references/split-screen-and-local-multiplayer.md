# Split-screen and local multiplayer

## Shipped examples

**Mario Kart 64 (1996).** Nintendo's manual documents independent race views for two to four local players. The three-player layout uses the fourth quadrant for information such as rankings or course radar. The manual also suggests watching opponents' screens while planning item use. [Nintendo instruction manual, pp. 14–15](https://www.nintendo.com/eu/media/downloads/games_8/emanuals/nintendo_8/Manual_Nintendo64_MarioKart64_EN.pdf)

**It Takes Two (2021).** Hazelight presents its two playable characters in split screen. Its audio designers discuss keeping the mix clear and spatializing sounds when two perspectives share an output. [EA game page](https://www.ea.com/en/games/it-takes-two/it-takes-two) · [Anne-Sophie Mongeau and Joakim Enigk Sjöberg, GDC, Creating the Soundscape of It Takes Two](https://www.gdcvault.com/play/1027157/Creating-the-Soundscape-of-It)

## Problem and design reasoning

Multiple people need simultaneous control and enough information to play on one machine. Independent views support diverging routes, while the smaller panels put pressure on readability and the extra views increase rendering work. Nintendo explicitly frames visibility into competing views as useful in its racing rules. Hazelight identifies mix clarity across different positions as an audio design problem. A transferable inference is that a shared view fits when important action stays within one readable frame, while separate views fit when players need independent perspectives. Either layout needs explicit ownership of input, UI focus, and audio cues.

## Techniques and practical checks

### Bind devices to player seats

Represent each seat independently of the physical controller. Give each seat its own control context, viewport, focus and join state. Define which controls open shared pause menus and what happens to a seat when its device disconnects or the application loses focus.

**Check** join two seats, disconnect one controller during a held action, and reconnect using a different device. Verify that the other seat retains control and that the returning seat resumes without replaying stale inputs or inheriting another seat's menu focus.

### Give each viewport a framing and readability budget

Choose layouts by player count and display shape. Specify field of view or camera bounds against each panel's aspect ratio, scale local HUD elements relative to their own viewport, and reserve shared information for a global overlay or unoccupied area.

**Check** every supported player count on the smallest target display. Separate players to the farthest legal distance and place essential prompts at the edges. Verify that each panel shows its own action and prompts without cropping or leaking controls to another panel.

### Measure worst-case viewpoints

Share simulation where game rules allow it and profile per-view work separately. Track render targets, culling, post-processing, and duplicated shadow or transparency work. Choose lower-cost effects per viewport if captures show that the additional camera threatens the platform's frame budget.

**Check** one camera, then maximum local seats aimed in the same direction, then each aimed at a different dense region. Compare frame-time tails and memory peaks, including an effects-heavy encounter and join or leave transitions.

### Resolve shared audio explicitly

Classify sounds by who needs to hear them. Choose how one shared output represents listener-relative cues and avoid replaying the same world event once per view at full volume. Give critical prompts a clear visual source when panning alone cannot distinguish the seats.

**Check** trigger one shared world sound and different private cues for both players simultaneously, then swap their viewport positions. Listen on stereo and mono, verify the shared sound does not double in level, and ask both players to identify their own cues.
