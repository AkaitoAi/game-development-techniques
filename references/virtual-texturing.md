# Virtual texturing

## Problem and design reasoning

A large world may require more uniquely detailed texture data than can fit in GPU memory. Streaming whole textures can still load regions that the camera never sees. Virtual texturing divides virtual mip levels into pages so a bounded physical cache can retain the useful regions. In *Far Cry 5*, Ubisoft also cached expensive terrain material blends in those pages. The inference for another project is that this trade pays off when texture capacity or repeated material evaluation is the real bottleneck. It adds address translation, residency management, and page production costs. [Ubisoft, Terrain Rendering on Zeta, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/TerrainRenderingFarCry5.pdf)

## Shipped examples

- *RAGE* used software virtual textures to work with large, detailed texture data. id Software's SIGGRAPH 2012 presentation, **Virtual Texturing in Software and Hardware**, explains its virtual addressing and filtering. [J.M.P. van Waveren and collaborators, SIGGRAPH 2012](https://mrelusive.com/publications/presentations/2012_siggraph/Virtual_Texturing_in_Software_and_Hardware_final.pdf)
- *Far Cry 5* looked up terrain material data through a page table and generated cached pages from terrain layers. Ubisoft documented bounds on per-frame page generation and described collecting visible page requests through GPU feedback and CPU readback. This is an example of generating page contents at runtime instead of loading all page payloads from disk. [Ubisoft, Terrain Rendering on Zeta, GDC 2018](https://media.gdcvault.com/gdc2018/presentations/TerrainRenderingFarCry5.pdf)

## Techniques and practical checks

1. **Define the virtual and physical layouts.** Identify a requested page by texture, mip, and tile coordinates. Store its residency mapping separately from the physical page payload. Keep an available coarser mip as fallback.  
   **Check** by clearing the fine-page cache and moving the camera toward a high-contrast surface. Confirm that it remains correctly mapped while detail resolves and that the physical memory budget stays bounded.
2. **Preserve sampling across page boundaries.** Derive the intended mip and anisotropy from the virtual footprint. Use neighboring texels in page borders or a sampling scheme that handles cross-page filtering. Apply consistent residency across material layers that must agree.  
   **Check** a diagonal high-frequency pattern crossing multiple tiles at shallow angles and across mip transitions. Inspect the mapping and final output for seams, discontinuities, and mismatched normal or color detail.
3. **Turn feedback into a bounded request queue.** Deduplicate page requests, discard outdated demand when appropriate, and budget work for I/O or page generation, upload, and mapping updates. Reserve enough capacity for coarse coverage and prioritize visible detail. Publish a new mapping only when its payload can be sampled safely.  
   **Check** by teleporting between two areas and then reversing direction. Track requested pages, miss rate, queue age, fallback duration, and per-frame upload or generation cost.
4. **Test eviction and content changes.** Avoid reusing a physical slot before references to its prior contents have retired. For generated pages, invalidate affected regions and related mips when source layers change.  
   **Check** with a deliberately small cache and repeated edits to a terrain decal or material layer. Look for stale content, wrong-page flashes, and sustained churn. Compare the same workload against ordinary streaming or direct material evaluation.
