# Runtime animation retargeting

## Problem and design reasoning

Characters with different proportions can share a movement intention while needing different joint poses. Copying rotations alone may produce foot sliding, unreachable hand targets, or an incorrect root trajectory. Naughty Dog's shipped *Uncharted 2* character pipeline describes a shared skeleton across character variants and helper joints for deformation. That gives a concrete production example of standardizing the animation contract. [Judd Simantov and Christophe Desse, Uncharted 2 Character Pipeline, GDC 2010](https://www.gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An)

Ubisoft's Alexander Bereznyak presented IK Rig as a way to convert animation into IK chains, adjust them for context, and convert the result to another rig at runtime or offline. This is evidence for the technique's proposed capability. It does not establish that a particular shipped Ubisoft game used every part of it. The implementation choices below are portable recommendations. [Alexander Bereznyak, IK Rig: Procedural Pose Animation, GDC 2016 slides](https://media.gdcvault.com/gdc2016/Presentations/Bereznyak_Alexander_IKRig_Moving_Forward.pdf)

## Techniques and practical checks

### Compile a rig mapping before playback

Map semantic chains, root, pelvis, end effectors, and optional twist joints to stable joint IDs. Record rest transforms, axis conventions, scale units, and which joints may translate. Validate missing or ambiguous mappings before making the character playable.

**Check** load a valid rig, one with a missing optional twist joint, and one with a missing required foot. Verify supported fallbacks and a clear diagnostic for an unusable mapping.

### Preserve trajectory and contact separately

Choose whether the source root path is scaled, preserved, or supplied by gameplay. Retarget the pose relative to that trajectory and solve planted contacts against the target rig's dimensions. Make the movement owner explicit before correcting foot or hand placement.

**Check** compare short and tall variants on the same start, stop, turn, and step traces. Measure root displacement and planted-foot drift, then verify collision still follows the intended movement owner.

### Bound unreachable targets

Set chain limits and a policy for targets beyond reach, such as controlled pelvis adjustment or a reduced contact correction. Blend solver influence at entry and exit, and report persistent constraint error for authoring review.

**Check** drive a hand target through the reach limit and move a foot target above a knee-height obstacle. Verify bounded joints, continuous motion, and a recoverable pose after the target returns.

### Cache immutable mapping data

Share validated rig metadata while keeping solver scratch and contact state per character. Invalidate the mapping when the skeleton revision changes. Evaluate retargeting before layers that depend on final contact positions according to the pose pipeline's contract.

**Check** retarget several variants concurrently, swap a skeleton revision, and disable and re-enable a character. Verify independent contact state, correct invalidation, and stable cost under the intended character count.

See [Inverse kinematics](inverse-kinematics.md) for solver behavior and [Animation rigging](animation-rigging.md) for authored skeleton and deformation checks.
