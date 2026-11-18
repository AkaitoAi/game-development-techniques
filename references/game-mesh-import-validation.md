# Game mesh import validation

## Problem and design reasoning

A mesh can pass a visual check in its authoring tool and still arrive with different units, missing materials, incompatible skeleton references, or topology beyond the runtime's limits. Validation needs to inspect the imported artifact that gameplay will use. In *Quake III Arena*'s released renderer, model registration checks format identity and MD3 version, rejects models without frames, and checks surface vertex and index limits. This is evidence of concrete runtime asset constraints, while a complete modern import validator needs additional checks beyond that historical loader. [id Software, Quake III model registration and MD3 loading](https://github.com/id-Software/Quake-III-Arena/blob/master/code/renderer/tr_model.c)

Naughty Dog's GDC 2010 *Uncharted 2* character pipeline presentation describes sculpting, building arbitrary meshes, and sampling high-resolution detail onto the resulting mesh. It shows why the source geometry and the runtime representation have different roles. The validation manifest and fixtures below are portable recommendations for checking the contract between them. [Naughty Dog, Uncharted 2 Character Pipeline, GDC 2010](https://gdcvault.com/play/1012350/Uncharted-2-Character-Pipeline-An)

## Authoring and implementation checks

### Define the imported asset contract

Record the expected units, axes, asset class, required channels, material slots, skeleton, sockets, and platform limits. Express important requirements as importer diagnostics with asset identity and a correction. Allow intentional exceptions through explicit asset settings.

**Check** import disposable fixtures with a unit mismatch, missing UV channel, absent material, and excessive vertex count. Confirm each fails or warns at the configured stage with an actionable location. Verify a valid exception is reproducible in a clean import.

### Inspect the generated representation

Validate finite vertex attributes, index ranges, bounds, submesh assignments, skin weights, bone references, and imported hierarchy as appropriate to the asset class. Compare source expectations against the generated asset after import conversion, mesh splitting, and optimization.

**Check** import a skinned character, a modular prop, and a large mesh that triggers the engine's splitting or index-format policy. Confirm their imported counts, bounds, materials, and skeleton references match the manifest. Reject invalid indices and nonfinite attributes before they reach rendering or physics.

### Exercise consumers together

Load the mesh through a representative prefab or entity that uses its collision, animation, materials, sockets, and LODs. A mesh validator should surface missing dependencies, while runtime fixtures confirm that those dependencies agree in play.

**Check** run a motion range, attach equipment, switch LODs, and exercise collision in the integration scene. Capture a reference camera and compare silhouette, contact, shading, and attachment positions against tolerances chosen for the asset.

### Preserve reproducible import inputs

Keep source settings, tool versions, dependencies, and output diagnostics available for a build. Include changes to those inputs in cache invalidation. Compare a clean conversion with an incremental conversion after changing a shared skeleton or material.

**Check** build the same fixture from a clean checkout and a warm cache, then change one dependency. Verify the affected artifacts rebuild and the packaged build contains the expected revision. Compare meaningful geometry and dependency data when files contain incidental metadata.

See [Game asset scale conventions](game-asset-scale-conventions.md), [Mesh normal and tangent validation](mesh-normal-and-tangent-validation.md), and [Collision proxy authoring](collision-proxy-authoring.md) for focused validators.
