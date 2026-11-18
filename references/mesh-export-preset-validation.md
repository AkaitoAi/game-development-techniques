# Mesh export preset validation

## Problem and design reasoning

An artist's export dialog can silently change axes, geometry selection, triangulation, or skeleton contents between revisions. Reproducible exports need an explicit contract between source authoring and runtime consumption. Judd Simantov's GDC 2010 *Uncharted 2 Character Pipeline* materials show separate game meshes, base sculpt meshes, and high-resolution sculpts. That shipped production example establishes that multiple source representations serve different purposes. It does not document the preset system recommended here. [Developer presentation](https://media.gdcvault.com/gdc10/slides/simantov_judd_uncharted2_character_pipeline.pdf)

Tool boundaries introduce additional constraints. Epic's FBX static mesh documentation identifies exporter/importer version compatibility, pivot behavior, and alternative triangulation stages. These are concrete examples of settings a pipeline must make deliberate. The portable reasoning is to version the export recipe with its intended asset class and verify the produced file, so a clean build can reproduce an artist's accepted result. [Epic, FBX Static Mesh Pipeline](https://dev.epicgames.com/documentation/en-us/unreal-engine/fbx-static-mesh-pipeline-in-unreal-engine)

## Techniques and practical checks

### Version an export recipe per asset class

Store format and exporter version, units, axes, selection rules, modifier evaluation, geometry channels, triangulation stage, and animation policy. Separate static props, skinned characters, collision proxies, and animation clips when their contracts differ. Make supported tool versions and permitted exceptions explicit.

**Check** export one fixture per asset class from a fresh authoring-tool session. Confirm the recipe produces the required channels and contents without relying on a user's remembered dialog state.

### Validate the selected source set

Select export roots by persistent metadata or a deliberate manifest. Check required descendants, hidden helpers, collision objects, skeleton membership, and output names before exporting. Avoid accidental inclusion of sculpt sources, references, or unrelated scene contents.

**Check** add an unrelated visible mesh, hide a required child, and duplicate an export name. Confirm the preflight either produces the intended artifact or reports an actionable failure before overwriting an accepted output.

### Test conversion with asymmetric fixtures

Use a labeled asymmetric mesh with known dimensions, noncentral pivot, mirrored element, UV channels, and hard edges. Include a skeleton fixture where relevant. Inspect the exported artifact and a representative runtime consumer to expose conversion errors.

**Check** verify axis labels, dimensions, pivot motion, winding, channel retention, and material assignments after conversion. Compare each against numeric tolerances and reference images established for that recipe.

### Record provenance and compare meaningful output

Record source revision, recipe revision, exporter version, dependencies, and diagnostics beside the artifact or in build metadata. Compare geometry and channel data when file timestamps or incidental metadata prevent byte equality. Require reviewed baseline updates when recipe changes intentionally alter output.

**Check** export the same fixture twice, then on another supported workstation. Confirm meaningful data matches. Change one recipe setting and verify provenance and the expected output difference identify the cause.

See [Game mesh import validation](game-mesh-import-validation.md) for validation after import.
