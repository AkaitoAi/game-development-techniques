# Mesh baking workflows

## Problem and design reasoning

High-detail art and a low-detail game mesh can each look correct while their projected bake contains skew, ray misses, contamination from neighboring pieces, or seams after import. This guide covers projection and reproducibility between those assets. See [Mesh normal and tangent validation](mesh-normal-and-tangent-validation.md) for the runtime shading-basis contract.

Naughty Dog's *Uncharted 2* character pipeline sampled sculpted detail onto its game mesh and addressed UV-border seams. The GDC presentation documents that pipeline stage. The recommendations below make projection inputs and acceptance tests explicit so changes to the game mesh can be rebaked consistently. [Richard Diamant and Judd Simantov, Uncharted 2 Character Pipeline, GDC 2010](https://gdcvault.com/play/1012552/Uncharted-2-Character-Pipeline-An)

Epic's FBX pipeline documentation describes generating normal maps from high-resolution geometry and recommends controlling triangulation before import. It provides a technical contract, while the *Uncharted 2* talk provides shipped-game context. [Epic Games, FBX Static Mesh Pipeline](https://dev.epicgames.com/documentation/en-us/unreal-engine/fbx-static-mesh-pipeline-in-unreal-engine)

## Techniques and practical checks

### Freeze the bake inputs

Record the high mesh, final triangulated game mesh, smoothing boundaries, UV channel, normal orientation, tangent convention, and baker settings. Treat these inputs as a versioned unit. A geometry or UV change should invalidate maps that depend on it.

**Check** rebuild a representative asset from recorded inputs in a clean workspace. Compare map dimensions, channel interpretation, triangle topology, and appearance under the same runtime light.

### Control projection coverage

Inspect cages or ray distances against the actual forms. Use explicit mesh pairing or temporarily separated pieces when nearby surfaces contaminate one another. Keep the projected detail consistent with the final assembled asset.

**Check** bake fixtures containing a deep recess, thin gap, hard corner, and closely spaced parts. Visualize misses and inspect skew or transferred detail before applying materials.

### Match padding to the consumer

Set resolution and island spacing from the smallest useful runtime mip and expected viewing distance. Provide dilation outside islands and align the bake with the compression and sampling path used by the project.

**Check** inspect the asset at successive mips in motion, including UV seams and mirrored regions. Compare uncompressed and shipping-compressed results to identify whether a defect originates in projection, filtering, or compression.

### Review in an integration scene

Use a simple reference material and a rotating light before judging the final textured result. Then check the asset under shipping exposure, animation if applicable, and representative LODs.

**Check** inspect a known flat region, silhouette transition, and UV border in the packaged build. Re-export one changed low mesh and verify that the pipeline catches the stale bake or produces the updated maps.
