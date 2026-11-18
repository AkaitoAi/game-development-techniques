# Enemy telegraph design

## Problem and design reasoning

A player can know an enemy's rules and still fail to recognize which attack is coming when its pose overlaps scenery, the camera obscures it, or other effects mask the cue. This guide focuses on the information available before an enemy commits to an attack. See [Enemy design](enemies.md) for roles and [Encounter role composition](encounter-role-composition.md) for combined pressure.

In *Returnal*, Housemarque assigned Hyperion's projectile colors to different attack behaviors and reaction demands. The developers tested layered patterns for readability and required a successful damage-free performance before accepting an iteration. Those are documented production constraints. The recommendation below is to validate recognition separately from execution, so a long warning is useful only when players can interpret it. [Housemarque, Hyperion development account](https://blog.playstation.com/2021/05/28/returnal-the-making-of-that-unforgettable-hyperion-fight/)

Mihir Sheth's GDC talk describes adapting *God of War*'s combat to a close player-controlled perspective. That change affects which threats fit within the frame. The portable inference is to define an attack cue together with its expected camera and player response. [Mihir Sheth, Evolving Combat in God of War for a New Perspective, GDC 2019](https://media.gdcvault.com/gdc2019/presentations/Sheth_Mihir_EvolvingCombat.pdf)

## Techniques and practical checks

### Define the question each cue answers

Specify the intended response, including whether the player should dodge, block, reposition, or interrupt. Give distinct responses distinguishable pose, motion, sound, or shape cues. Keep the first recognizable moment separate from the first animated frame.

**Check** show short clips ending before impact to unbriefed players. Ask them to identify the threatened area and intended response. Record recognition errors independently of later action timing.

### Budget recognition and execution together

Measure the time from the recognizable cue to the last successful response input, including animation recovery and input constraints. An enemy tracking the player during its wind-up needs an explicit commitment point so the displayed direction remains meaningful.

**Check** place inputs around the final valid response boundary at supported frame rates. Test an attack during player recovery and verify that the stated response is actually available.

### Preserve cues under encounter load

Reserve visual and sonic space for critical warnings. Use contrast, silhouette, timing, and distinct shapes where color alone can be confused. Include the least favorable supported camera direction and the project's reduced-effects settings.

**Check** repeat a mixed-enemy encounter with dialogue, foreground particles, camera shake, muted audio, and reduced color discrimination. Count missed cue recognition and compare against the isolated attack.

### Revalidate variants and interruptions

Treat faster difficulty variants, phase changes, cancelled attacks, and resuming from a stun as changes to the cue contract. Give feints learnable rules and ensure a cancelled attack retires any obsolete warning.

**Check** trigger stun, death, target loss, and phase change during wind-up. Verify that the player sees the current threat and that a retired telegraph produces no unexplained hit.
