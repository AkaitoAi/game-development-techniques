# Texture atlasing and batching

## Problem and design reasoning

Many small textures can cause frequent binding changes, while many small draws can consume CPU submission time. Packing content into shared pages can reduce binding changes. Draw batching requires compatible render state, compatible geometry submission, and legal ordering as well. Separate these costs in captures before choosing a packing policy.

## Shipped examples

- **QuakeWorld, OpenGL client.** id Software's released renderer packs surface lightmaps into shared 128×128 pages. Its allocation routine returns the page and origin for a rectangle, and the renderer uses that allocation to build and sample each surface's lightmap. This is evidence for shared texture allocation. Any benefit to draw batching in a different renderer depends on its submission path. [id Software renderer source](https://github.com/id-Software/Quake/blob/master/QW/client/gl_rsurf.c)
- **Night in the Woods, iOS.** Secret Lab's GDC 2018 talk describes an atlas tool built during the port. It split sprites into pieces, skipped transparent pieces, and packed retained pieces to improve atlas usage. Secret Lab separately confirmed that the iOS port shipped. The transferable inference is to compare texture savings against the extra sprite geometry used to reconstruct individual frames. [Jon Manning, GDC 2018 session](https://gdcvault.com/play/1025419/By-Grabthar-s-Hammer-What) · [GDC 2018 slides](https://media.gdcvault.com/gdc2018/presentations/Manning_Jon_By_Grabthars_Hammer%20copy.pdf) · [Secret Lab release update](https://www.patreon.com/secretlab/posts/even-more-news-55979375)

## Techniques and practical checks

- **Pack by co-use and texture requirements.** Group assets that appear or load together and share compatible format, sampler, and mip requirements. Store stable asset IDs, page locations, UVs, and original offsets. **Check** a representative level transition and a menu overlay. Compare active page count, peak residency, and texture binding changes before and after packing.
- **Protect sampling boundaries.** Pad packed regions and extend border texels as needed for the filter footprint and mip chain. Preserve pivot and source offset when trimming or dicing sprites. **Check** adjacent high-contrast sprites at fractional scales, during movement, and through the smallest supported mip. Inspect for bleeding and alignment shifts.
- **Batch legal runs in draw order.** Include pass, material state, page, blending, and clipping in the batch key. Merge compatible opaque items when allowed and preserve ordered transparent composition. **Check** overlapping translucent elements with interleaved materials and clip regions. Compare pixels against the unbatched render and inspect event order in a frame capture.
- **Measure total throughput.** Compare texture binds, draw calls, CPU submission time, GPU work, peak memory, and upload cost on the target device. Set page sizes based on observed residency. **Check** both a sprite-dense scene and one with sparse use of many pages, then reload from cold. Confirm that a submission win fits the texture and streaming budgets.
