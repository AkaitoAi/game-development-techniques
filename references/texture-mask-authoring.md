# Texture mask authoring

## Problem and design reasoning

A packed image can look plausible while its channels drive the wrong material properties. Color decoding, compression, and mip generation can alter scalar meaning. Treat a mask as a versioned data contract between authoring, import, and shading.

David Neubelt and Matt Pettineo's GDC 2014 talk on *The Order: 1886* describes material inheritance and compositing intended to accelerate texturing and maintain consistent response across production content. This establishes the production motivation for shared material semantics, without proving any particular channel layout. [Crafting a Next-Gen Material Pipeline](https://www.gdcvault.com/play/1020485/Crafting-a-Next-Gen-Material)

Epic's official mask documentation demonstrates packing separate properties into RGBA channels and using a mask to restrict emission. Its starter-chair layout is an example rather than a universal convention. The recommendations below transfer the principle to any engine. [Epic Games, Using Texture Masks](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-texture-masks-in-unreal-engine)

## Techniques and practical checks

### Publish the channel contract

Record each channel's meaning, value range, default, inversion, UV set, and encoding. Name the contract and version it alongside shaders and export presets. Decide whether layers are independent overlays or mutually exclusive weights. Specify behavior when a map or channel is absent.

**Check** use a diagnostic texture with a different recognizable pattern in every channel. Confirm the shader's isolated previews match the documented assignments and defaults.

### Keep scalar data linear

Disable display-color decoding for scalar masks. Do not pack display-encoded color with linear scalars unless the pipeline explicitly handles their different interpretation. Pack only maps that can share resolution, sampling, UVs, and residency behavior without wasting more than the combination saves.

**Check** compare known 0, 0.5, and 1 values before export and after shader sampling. Validate imported metadata and test missing alpha on the cooked format.

### Preserve the intended boundary

Define what filtering should mean for each channel. Soft blends, binary cutouts, identifiers, and normalized weights need different treatment. A categorical identifier should not silently become an intermediate category through filtering. Pad UV islands and review compressed mips rather than only the source image.

**Check** inspect close, distant, and grazing views in motion. Look for layer bleed, lost cutout coverage, unstable boundaries, and weight sums outside the chosen rule.

### Measure the cooked packing choice

Compare packed and separate alternatives on target hardware. Include alpha-related storage changes, compression error, and samples actually eliminated. Automate contract validation when exporters or shaders change.

**Check** cook a high-contrast fixture on each supported texture format. Compare individual channel error, resident bytes, and GPU cost, then deliberately load a mismatched contract version and verify a clear diagnostic.

See [Materials and texture pipelines](materials-and-textures.md) for broader sampling and residency decisions and [Game material authoring validation](game-material-authoring-validation.md) for reference-scene evaluation.
