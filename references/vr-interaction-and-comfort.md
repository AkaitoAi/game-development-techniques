# VR interaction and comfort

## Problem and design reasoning

**Problem.** Players cannot reliably reach objects or avoid a route because the motion feels uncomfortable. Test seated and standing use, tracking loss, and changes in viewpoint separately.

**Design reasoning.** Valve describes testing locomotion prototypes before shipping several modes in Alyx. An inferred priority is to preserve progression across movement options so players can choose the comfort and control trade-off that suits them. See the shipped examples below for sources.

## Shipped examples

**Lone Echo (2017).** Ready At Dawn built zero-gravity traversal around gripping the environment and pushing or pulling with tracked hands. The player's own hand movement provides a reference for the resulting motion. [Jacob Copenhaver, GDC 2017](https://www.gdcvault.com/play/1024240/It-s-All-in-the)

**Half-Life: Alyx (2020).** Valve supported blink teleport, shift teleport, and continuous locomotion. The development team compared prototypes with different motion and interface behaviors before settling on the shipped options. [Valve locomotion talk](https://www.youtube.com/watch?v=TX58AbJq-xo)

## Techniques and practical checks

### Support distinct movement preferences

Offer a discontinuous movement option and tune continuous movement independently when the design supports both. Show a destination and facing preview before teleporting. Assess clearance for body and head, account for tracking offsets, and require a clear confirmation gesture. Provide snap turning and configurable turning where appropriate.

**Check** the same route using each supported movement mode. Include a low doorway, a moving surface, a narrow landing, and a turn immediately after arrival. Verify that each mode can complete the encounter and that the preview agrees with the final pose.

### Keep physical tracking and game motion separate

Compose the tracked head pose with the locomotion rig. Treat camera rotation, head wobble, and forced displacement as distinct decisions. When players lean through game geometry, preserve tracking and use a visual obstruction treatment or repositioning policy that preserves orientation. Design transitions with a stable reference, predictable motion, or a fade.

**Check** crouching, leaning, recentering, and shifting from standing to seated height. Verify consistent head movement through pauses, scene transitions, and tracking recovery. Check the actual headset view rather than relying on the desktop mirror.

### Make hand interaction readable and reachable

Give each object hand targets, an ownership rule, and clear affordances. Show which target will be grabbed before commitment when targets cluster. Offer range assistance or an alternative input for required actions that exceed a comfortable reach. Decide how grab and release behave when a controller loses tracking.

**Check** with both dominant-hand settings, seated play, limited reach, and two hands targeting nearby objects. Confirm that required actions can be completed without reaching through a boundary and that a lost controller can recover cleanly.

### Validate comfort on target hardware

Measure delivered frame timing in representative gameplay. Test long traversals, dense scenes, and repeated transitions on each supported refresh-rate tier. Collect feedback from players with varied VR experience and comfort preferences, using optional stop points and short sessions.

**Check** frame-time percentiles and missed presentation deadlines for a reproducible route. Record the movement mode, device, and action preceding each reported issue. Re-run the same route after adjusting the relevant motion or performance setting.
