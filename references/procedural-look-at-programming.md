# Procedural look-at programming

## Problem and design reasoning

A character looking toward a moving subject can twist its neck, fight an authored action, or snap when its target disappears. This guide focuses on target arbitration and gaze pose composition. For general chain solving, see [Inverse kinematics](inverse-kinematics.md).

For shipped *Uncharted: Drake's Fortune*, Christian Gyrling described testing procedural neck and spine IK, partial poses, and additive animation for look-at and aim. The team found unwanted neck deformation in low poses and stiffness with partial animation. Additive poses preserved the base motion and gave animators control over the result. Those observations support choosing a pose strategy against actual character poses and deformation limits. They do not prescribe one solver for every game. [Christian Gyrling, Creating a Character in Uncharted: Drake's Fortune, GDC 2008, slides 42–45](https://media.gdcvault.com/gdc08/slides/S6169i1.pdf)

Naughty Dog's later presentation covers animation and player control across the first two *Uncharted* games. The contracts below are portable recommendations for integrating gaze with that kind of character control. [Naughty Dog, Animation and Player Control in Uncharted, GDC 2010](https://www.gdcvault.com/play/1012451/Animation-and-Player-Control-in)

## Techniques and practical checks

### Give gaze a target owner

Represent a request with target identity or world anchor, purpose, priority, expiry, and weight. Resolve competing requests before pose evaluation. Specify when aiming, conversation, interaction, and idle attention yield to one another. Revalidate a target when its request is consumed.

**Check** alternate an idle interest point, conversation target, and aim request while destroying one target. Verify the selected owner and expiry in a trace and confirm a retired target cannot pull the character's gaze.

### Constrain the direction in the rig's space

Convert the target into the defined reference frame and derive bounded yaw and pitch. Handle a target coinciding with the gaze origin and one passing behind the character. Distribute permitted rotation across eyes, head, neck, and torso according to the rig's authored limits.

**Check** move a target around a crouched character with a rotated parent, including directly above, behind, and at the origin. Inspect deformation, joint limits, finite rotations, and continuity at the rear boundary.

### Compose with authored action ownership

Apply gaze at a documented stage in the pose pipeline and assign masks or weights for actions that need the same joints. A procedural solver or additive pose can both work, provided the result respects animation direction and reach constraints. Keep eyes and head coordinated without forcing identical response speeds.

**Check** look at a moving target during reload, recoil, a two-handed interaction, and a transition to ragdoll. Compare the pose before and after gaze evaluation and confirm important hand contacts survive.

### Make acquisition and release deliberate

Smooth target motion and blend influence using the character's clock policy. Define immediate resets for teleports and rig replacement. Add target-selection hysteresis where equally ranked candidates would otherwise cause visible oscillation.

**Check** cross two candidate targets, pause and resume, teleport the character, and return it to a pool with gaze active. Verify bounded switching, release to the expected base pose, and no old request affecting the reused character.
