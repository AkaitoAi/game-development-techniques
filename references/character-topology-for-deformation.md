# Character topology for deformation

## Problem and design reasoning

A character can look correct in a neutral pose but lose volume or produce shading breaks when a shoulder twists, a knee bends, or a face changes expression. Topology needs to support the intended movement, skinning, and character variation. [Animation rigging](animation-rigging.md) covers rig behavior and [3D modeling](3d-modeling.md) covers the wider asset workflow.

For *Uncharted 2*, Naughty Dog standardized the base portion of its head mesh to speed game-mesh creation and allow weighting transfer across main characters. The developer presentation also covers shared skeletons, clothing variations, and seam problems. A transferable inference is to preserve compatible topology where shared deformation work benefits the asset family, while testing each character's actual proportions. [Richard Diamant and Judd Simantov, Uncharted 2 Character Pipeline, GDC 2010](https://gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An) · [Naughty Dog presentation slides](https://media.gdcvault.com/gdc10/slides/simantov_judd_uncharted2_character_pipeline.pdf)

Wasim Khan's GDC 2021 talk on the shipped *The Last of Us Part II* describes character technology developed around frequent reviews and playtests. That production constraint supports evaluating topology through repeatable poses early enough to revise it. The specific deformation tests below are recommended procedures. [Wasim Khan, Character Technology of The Last of Us Part II, GDC 2021](https://www.gdcvault.com/play/1027376/Character-Technology-of-The-Last)

## Techniques and practical checks

### Allocate geometry around the motion envelope

Identify expected joint angles and silhouette changes before final topology. Place sufficient vertices across bending regions and inspect how edge flow affects compression, stretching, and volume. Evaluate the exported triangulation with the actual skinning method.

**Check** replay deep knee and elbow bends, shoulder elevation, wrist twist, and the production character's extreme poses. Inspect silhouette, penetrations, and shading from gameplay camera distances and close-ups the game supports.

### Validate reusable topology against proportions

When transferring weights or deformation targets, document which vertex identities, bind poses, and regions must remain compatible. Verify each new proportion set instead of assuming shared connectivity guarantees identical deformation.

**Check** transfer the approved setup to the shortest and longest supported limb proportions and a distinct head shape. Run the same pose suite and compare contacts, volume, and facial landmarks.

### Coordinate seams with deformation and shading

Place modular boundaries where the asset system can preserve corresponding positions, compatible normals, and intended weight behavior. Check outfit variants and avoid using neutral-pose overlap alone as proof that a seam will remain hidden.

**Check** combine supported body and outfit variants, then run twists and extreme bends under grazing light. Inspect cracks, double surfaces, exposed interiors, and normal discontinuities in the player build.

### Test reduced geometry with the same pose suite

Create LODs with a deformation budget as well as a triangle budget. Preserve enough structure around moving joints and facial features for the promised viewing distance. Revalidate weight transfer and authored corrective shapes after topology changes.

**Check** force each LOD through identical recorded poses, then move the camera across switching thresholds. Record visible joint collapse, silhouette jumps, and seam changes, together with measured render and animation cost.
