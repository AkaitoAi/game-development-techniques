# Mesh material slot validation

## Problem and design reasoning

A mesh can have correct textures and shaders yet put skin on clothing after reimport, show a different assignment at a distant LOD, or retain another character's override. Material assignments are an asset interface: face groups, semantic slot names, imported sections, and runtime replacements must agree. Use material authoring validation for shader inputs and this guide for which surfaces receive those materials.

**Uncharted 2** is a useful production context. Richard Diamant and Judd Simantov's GDC 2010 talk, **Uncharted 2 Character Pipeline: An In-depth Look at the Creation of U2's Characters**, describes efficiently producing mesh varieties and maintaining a shared skeleton across outfits. This demonstrates why variant consistency matters; the public summary does not establish Naughty Dog's material-slot protocol. The validation contract below is a portable recommendation. [Naughty Dog, GDC session](https://www.gdcvault.com/play/1012798/Uncharted-2-Character-Pipeline-An)

Epic's FBX documentation supplies a concrete import example: multiple materials cover different mesh faces, original material names help match slots during reimport, and resolving materials by name avoids dependence on numeric ordering. These behaviors are importer-specific; confirm the equivalent contract in the target engine and DCC tool. [Epic, FBX Material Pipeline](https://dev.epicgames.com/documentation/unreal-engine/fbx-material-pipeline-in-unreal-engine?lang=en-US)

## Techniques and practical checks

### Give slots semantic identities

Define required and optional meanings such as body, cloth, eyes, and detachable equipment. Keep display labels separate from stable identifiers if renaming is common. Record expected face groups, allowed empty assignments, and the intended fallback for a missing material. Avoid assuming every mesh has identical numeric indices.

**Check** assign conspicuous diagnostic colors to each semantic slot and inspect every surface, including backfaces visible in gameplay. Fail validation for unknown assignments, unintended empty sections, or required groups with no faces; report the asset and offending group.

### Make reimport an explicit regression case

Capture a mapping from semantic slot to imported section and material reference. Reimport after changing source material order, adding a material, and changing a display name. Compare intended assignments and instance overrides rather than only checking that import succeeds. Decide whether renamed semantics require migration or are a breaking change.

**Check** run those edits on a fixture containing at least two visibly different face groups. Verify assignments survive the supported edits, unsupported edits produce useful errors, and reverting the source returns a stable result without growing unused slots.

### Validate every LOD and mesh variant

LODs may merge sections or intentionally use fewer materials. Store an explicit mapping when they do; do not require identical counts unless the runtime requires them. Include outfit, damaged, skinned, and assembled variants in validation. Review seams and hidden parts under the actual swap conditions.

**Check** force each LOD and variant while diagnostic materials and real materials are applied. Confirm semantic equivalence, expected section count, no exposed unassigned surfaces, and correct behavior when an optional part is absent.

### Rebind runtime overrides after mesh replacement

Resolve semantic identity against the current mesh revision. Invalidate cached numeric indices when that mesh changes. Track which subsystem owns temporary overrides such as damage or selection; restore the appropriate baseline on return to a pool. Reject delayed material loads that target an obsolete mesh generation.

**Check** swap meshes while a material load is pending, toggle a temporary effect, and reuse the object for a different variant. Confirm the new surface assignments remain correct, stale loads cannot overwrite them, and cleanup removes only the overrides the retiring owner created.
