# Animation

## Problem and design reasoning

**Problem.** Authored transitions fail at directions and speeds players actually request, or runtime pose selection introduces foot slip and costly coverage gaps.

**Design reasoning.** Naughty Dog describes adopting motion matching after production challenges. The practical inference is to reserve search-based selection for movement where responsive continuity earns its capture and query budget, and preserve timed authoring for deliberate beats.

## Shipped examples

**Quake (1996).** The released WinQuake software renderer selects a model's stored vertex pose using the entity frame. For grouped frames, it calculates a looping phase from client time and a per-entity offset, then selects a pose from the group's time intervals. The technique gives an asset its own cadence while allowing individual instances to use different phases. [id Software renderer source](https://github.com/id-Software/Quake/blob/master/WinQuake/r_alias.c#L604-L651)

**For Honor (2017).** Ubisoft describes motion matching in terms of searching recorded poses for material that fits the desired motion and continues smoothly from the current pose. Its technical demonstration searches animation data from For Honor. The team's research also describes the data capture and control work behind the system. [Ubisoft La Forge technical explanation](https://www.ubisoft.com/en-us/studio/laforge/news/VjEIwquaIyEZZSw5RZI0V/drecon-datadriven-responsive-control-of-physicsbased-characters) · [Ubisoft research paper](https://static-wordpress.ubisoft.com/montreal.ubisoft.com/wp-content/uploads/2020/07/09154101/Learned_Motion_Matching.pdf)

**The Last of Us Part II (2020).** Naughty Dog technical animators describe switching to motion matching and shipping with that system after production challenges. Their account documents the migration as a production decision as well as an animation technique. [Michal Mach and Maksym Zhuravlov, GDC 2021](https://gdcvault.com/play/1027378/Motion-Matching-in-The-Last)

**What changed.** A timed sequence selects a pose from an authored order. Motion matching can select among many recorded poses in response to player intent and the character's current motion. That flexibility adds capture, coverage, memory, and query costs. Timed sequences still fit short effects, sprite animation, and actions with deliberately authored beats.

## Techniques to try

### Author timed sequences and bound transitions

Represent poses or clips with explicit times, loop behavior, and allowed transitions. Express one-shot cues as crossings of authored time markers. Track which cues fired for each playback instance, account for wraps, and define when interruption cancels a remaining cue. Use fixed simulation timing for authoritative action windows and align visual cues to those windows.

**Check** by starting, jumping over a marker during a hitch, crossing a loop boundary, and interrupting before and after the marker. Check cue counts and ordering. Repeat at the lowest supported frame rate and at any supported playback speeds.

### Blend locomotion while tracking foot contacts

Select locomotion clips by speed and direction. Align shared contact phases before blending, and tune playback rate against world speed. Use brief foot placement correction during planted intervals if the terrain varies. Define whether animation or simulation owns displacement for each state, then resolve collision before displaying the final pose.

**Check** with starts, stops, reversals, tight turns, steps, slopes, and interrupted actions. Measure how far a planted foot travels in world space and how far the visual root deviates from the collision body. Inspect the same captures at normal speed and frame by frame.

### Layer secondary poses with explicit priorities

Use a base movement pose and a bone mask when an upper-body action can play while moving. Add small corrections for look direction or recoil after the action pose if the layers are compatible. Set limits on additive offsets and assign priorities for actions that need the same joints.

**Check** while running, aiming, reloading, turning, and entering a full-body interaction. Inspect hand placement, torso rotation, and the pose at each layer's entry and exit. Verify the base gait still matches movement speed when the action layer is active.

### Prototype motion matching around a small movement set

Capture or source sequences containing starts, stops, turns, and transitions. Sample candidate features such as future root trajectory, local joint positions, and foot velocities. Normalize features and choose weights that reflect the game's movement priorities. Search periodically for a nearby pose, keep advancing the current clip between searches, and penalize switches that would add discontinuity. Use authored transitions for cases missing from the database.

**Check** with repeatable input traces and log query cost, chosen frames, switch frequency, trajectory error, and foot slip. Compare against a simple blend system on the same routes. Add a missing trajectory and verify whether new capture data resolves the problem before increasing search complexity.

### Author and validate retargetable motion

Publish a reference pose, root orientation, unit scale, named semantic chains, and contact markers alongside each source clip. Resolve the target's axes and reference pose before transferring motion. Scale root displacement according to the intended gameplay speed and target proportions, then solve important contacts such as planted feet and prop grips on the target skeleton. Review the result in game with the motion actually used by the controller. Naughty Dog's talk on *The Last of Us* describes the pipeline from raw capture through curves and retargeting to character skeletons. [Naughty Dog, GDC 2015](https://www.gdcvault.com/play/1021854/contactUs)

**Check** the same walk, planted turn, and two-handed prop interaction on the source and the shortest and tallest supported target rigs. Compare root displacement and duration, measure foot slide and grip drift during the marked contact intervals, and inspect elbows and knees during transitions. Re-export and verify the packaged clip retains its contact markers and import settings.
