# Mobile texture formats

## Problem and design reasoning

Compressed distribution size and the GPU cost of a resident texture are separate budgets. A package can download quickly and still expand into costly textures at runtime when a GPU cannot sample its format. Target usable formats by device capability and select block sizes by asset role, particularly for UI alpha, normal maps, and scenic color textures. [Android texture compression targeting guide](https://developer.android.com/guide/playcore/asset-delivery/texture-compression)

**Shipped examples.** Cat Daddy’s case study covers *NBA 2K Mobile*, *NBA SuperCard*, and *WWE SuperCard*. The studio reported previously decoding some ASTC art on CPUs when devices lacked ASTC support. Format targeting let it deliver ASTC assets, including its UI art, to compatible devices. In *Delta Force*, Hang Jiao’s GDC 2025 talk describes compressing generated virtual-texture pages to ASTC 4×4 or 6×6 before writing them, motivated by mobile bandwidth and heat. **Inference.** Multiformat delivery and runtime page encoding solve different parts of the texture budget. Start by profiling which path costs your project. [Cat Daddy developer case study](https://developer.android.com/stories/games/cat-daddy) · [Hang Jiao’s GDC 2025 slides](https://media.gdcvault.com/gdc2025/Slides/Jiao_Hang_Delta%2BForce%2BPerformant.pdf) · [Delta Force release listing](https://play.google.com/store/apps/details?id=com.proxima.dfm)

### Verify format selection end to end

**Check** a clean distribution build on ASTC-capable hardware and fallback hardware. Inspect both the delivered asset set and the resource format created on the GPU. Confirm identical asset identities and visual content across the tiers, and time loading to detect accidental CPU decompression or transcoding. [Android texture compression targeting guide](https://developer.android.com/guide/playcore/asset-delivery/texture-compression)

### Grade block sizes by texture role

**Check** side-by-side captures of fine UI alpha, normal maps under moving light, flat gradients, and distant terrain at the actual viewing scale and mip range. Compare candidate block sizes against a reference. Track visible artifacts and resident bytes per role, then reserve denser formats for assets that need them. [Android texture optimization guide](https://developer.android.com/games/optimize/textures)

### Measure the largest residency transition

**Check** the largest scene swap, then force an asset reload or quality-tier change. Record peak CPU and GPU memory, including staging or temporary decode buffers, and compare with the steady-state total. Include mip overhead and force the fallback path to catch devices whose peaks differ from the primary path. [Cat Daddy developer case study](https://developer.android.com/stories/games/cat-daddy)

### Evaluate runtime encoding only when pages change

**Check** a route that continually reveals new generated texture pages. Compare uncompressed uploads with runtime block encoding on representative devices and record encoder time, copy cost, resident memory, and resulting image quality. Test the worst burst separately from the steady state to see whether the bandwidth reduction offsets extra work. [Hang Jiao’s GDC 2025 slides](https://media.gdcvault.com/gdc2025/Slides/Jiao_Hang_Delta%2BForce%2BPerformant.pdf)
