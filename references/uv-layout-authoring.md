# UV layout authoring

## Problem and design reasoning

An asset can have an attractive texture preview while its production UVs stretch detail, leak between islands at distance, or prevent shared materials from being exchanged. UV layout is a contract among geometry, texture purpose, filtering, and runtime shading.

For *Sunset Overdrive*, Morten Olsen describes a standardized UV arrangement that let environment artists swap compatible materials while keeping edge treatments aligned. This is a documented production choice for reusable city surfaces. The broader layout workflow below is a recommendation and applies to unique assets as well as shared sheets. [Morten Olsen, The Ultimate Trim, GDC 2015 slides](https://media.gdcvault.com/gdc2015/presentations/Olsen_Morten_TheUltimateTrim.pdf)

Epic's UV documentation describes separate channels for different uses, checker previews for density, and layout operations that either allow shared overlaps or repack islands without overlap. This supports choosing a layout from its actual consumer requirements. It does not establish the UV workflow of a particular shipped title. [Epic Games, UVs Category](https://dev.epicgames.com/documentation/en-us/unreal-engine/uvs-category-in-unreal-engine)

See [Trim sheet authoring](trim-sheet-authoring.md) for reusable strip layouts, [Mesh normal and tangent validation](mesh-normal-and-tangent-validation.md) for bake conventions, and [Materials and textures](materials-and-textures.md) for import semantics.

## Techniques and practical checks

### Define the purpose of each channel

Record which channel carries base surface textures, a bake, decals, or another consumer. Specify whether islands may overlap, repeat, mirror, or extend outside the primary tile. Set a texel density target from viewing distance and supported texture tiers. Reserve higher density for detail the player can resolve in the intended view.

**Check** import a representative asset with a numbered checker and visualize each consumed channel in the runtime material. Compare its density with adjacent assets. Run overlap and range checks appropriate to the channel and verify that intentional shared regions pass while accidental ones are reported.

### Place seams using both shape and use

Cut islands where distortion or shading requires separation and where the gameplay view can tolerate a seam. Flatten curved surfaces with a distortion preview. Preserve directional details such as grain, labels, and trim orientation. Inspect export triangulation and tangent conventions when mirrored or rotated islands affect normal maps.

**Check** inspect the asset from all supported close views under a moving light. Include a bent or deformed pose where relevant. Verify the checker retains usable proportions and the texture direction remains coherent. Compare the exported mesh with the authoring preview to locate introduced shading seams.

### Pack for filtering and the lowest supported tier

Choose island spacing and edge dilation from the texture's mip and compression behavior. Recheck padding after any resolution reduction or automatic repack. When islands intentionally share a region, verify that their material interpretation and visible wear are compatible.

**Check** preview near and distant views on the lowest supported texture tier with the shipping filtering mode. Move past high-contrast island boundaries and inspect bleeding, shimmer, and disappearing details. Revise padding or layout until the supported view envelope is acceptable.

### Preserve layout contracts through revisions

Store authoring settings and channel names with the asset pipeline. Treat a UV change as an invalidation of dependent bakes and textures when their mapping changes. For interchangeable material families, validate the shared layout with a small fixture library before publishing a new sheet or mesh revision.

**Check** repack one asset, regenerate its dependent maps, and compare the result with a clean rebuild. Swap every supported material family on the fixture. Confirm no stale bake remains, expected alignments hold, and the revision reports which downstream assets need rebuilding.
