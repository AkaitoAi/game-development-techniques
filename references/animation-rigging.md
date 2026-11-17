# Animation rigging and skin deformation

## Problem and design reasoning

A character can follow a clip correctly and still lose volume at a joint, wrinkle at a face seam, or move an attachment unexpectedly. A rig needs to serve animators in the authoring tool and produce stable motion in the game. Diagnose pose generation, skin weighting, and exported data separately by comparing the same pose at each stage of the pipeline.

## Shipped examples

**Uncharted 2: Among Thieves (2009).** Naughty Dog's pipeline presentation discusses facial rigging, use of one skeleton for male and female characters, and helper joints for deformation. Standardizing the skeleton can make it easier to share assets across character variants, while helper joints give artists additional control over deformation. Those are transferable trade-offs inferred from the presented workflow. [Judd Simantov and Christophe Desse, GDC 2010 session](https://www.gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An) · [Developer slides](https://media.gdcvault.com/gdc10/slides/simantov_judd_uncharted2_character_pipeline.pdf)

**The Last of Us Part II (2020).** Naughty Dog's developers describe redesigning their facial rig after *Uncharted 4* and discuss the fidelity needed for their characters. The character technology talk describes the studio's work on the game's character features. A portable inference is to prioritize expressive range and pose combinations that will appear at the intended camera distance, then verify them after runtime export. [Naughty Dog developer discussion](https://www.naughtydog.com/podcasts/thelastofus/en/ep-05) · [Naughty Dog character technology presentation, GDC 2021](https://gdcvault.com/play/1027376/Character-Technology-of-The-Last)

## Techniques and practical checks

### Specify the exported skeleton contract

Record joint IDs, parent relationships, rest pose, bind transforms, units, and which joints drive attachments. Where the runtime expects baked joint transforms, keep editor-only constraints and controls out of the export and bake their evaluated result. Version intentional changes to the shared skeleton.

**Check** import an exported rig and its identity and stress poses. Compare world-space joint and socket transforms with the authoring tool. Reimport after changing an animator control and verify that attachment names and root behavior remain stable.

### Test skinning at useful extremes

Test elbow and knee flexion, shoulder elevation, wrist twist, and combined bends where they occur in the game. Inspect weights, normalize them, and check runtime influence limits. Add helper joints or corrective shapes where the silhouette still fails under representative poses.

**Check** exercise a shared pose set on multiple body variants. Look for collapsed volume, flipped surfaces, detached clothing, and changes after weight pruning in a packaged player.

### Make facial controls composable

Build neutral, expression, speech, and eye-control tests. Decide how expression and speech share mouth controls, and set clear ownership for jaw movement, eye aim, and eyelids. Add corrective poses for combinations that expose gaps or intersections.

**Check** combine a smile and open vowel, an eye turn and blink, and speech during a head turn. Inspect close shots and supported facial detail tiers for lip gaps, eye intersections, flicker, and popping.

### Preserve behavior when reducing rig cost

Measure skinning and posing costs in the largest expected crowd at the intended camera distance. Simplify optional deformations at distant tiers while keeping required collision, attachments, and gameplay markers valid. Validate garment and equipment weights against each supported rig tier.

**Check** move a character with a visible attachment across tier boundaries in both directions while changing animation and visibility. Record frame cost and inspect attachment motion, silhouette changes, and event continuity.
