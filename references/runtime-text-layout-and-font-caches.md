# Runtime text layout and font caches

## Problem and design reasoning

Text can consume frame time even when the string looks unchanged. Measuring, shaping, line breaking, glyph generation, and uploading glyph images have different invalidation conditions. Treating them as one disposable result can create a spike when a menu opens or a language changes. Valve's Chris Green presented distance-field magnification for vector textures and effects at SIGGRAPH 2007; this supplies a rendering technique whose benefits depend on scale and edge quality. Maxis's shipped *SimCity* (2013) UI used HTML, CSS, and JavaScript, and Yee Cheng Chin's GDC talk discusses integration, memory costs, and iteration tools. A transferable implementation inference is to track layout and glyph-image costs separately and measure the actual screen transitions. [Valve publications, Chris Green's SIGGRAPH 2007 notes](https://www.valvesoftware.com/en/publications) · [Maxis, How to Implement AAA Game UI in HTML and JavaScript, GDC](https://www.gdcvault.com/play/1022054/)

## Techniques and practical checks

### Separate shaping, layout, and glyph-image keys

Include the font face and version, style, size or raster scale, locale or shaping configuration, and relevant string content in their respective keys. Include wrap width and line-breaking policy in the layout key. Keep color-only changes out of geometry invalidation when the renderer permits it.

**Check** change color, font size, available width, and language independently. Count reshaping, layout rebuilds, atlas updates, and draw submissions to confirm each change invalidates the intended work.

### Prepare representative glyph coverage

Warm common UI text and supported scripts using representative strings when opening a critical screen must be predictable. Keep a bounded dynamic path for player names and other unpredictable content. Define fallback fonts and missing-glyph diagnostics. Use distance fields where their rendering quality and cost suit the intended scales.

**Check** open a cold inventory, show a mixed-script name, resize a label, and switch to a language with a different script. Inspect missing glyphs, text edges, glyph-upload cost, and the first visible frame.

### Give cache entries valid lifetimes

Keep text geometry references valid while the renderer uses their atlas locations. When repacking or evicting glyphs, update dependent geometry or use stable indirection. Bound cache growth and define a fallback for a full atlas.

**Check** cycle through many unique names and supported fonts until eviction occurs. Retain an older label on screen and verify it remains correct. Record memory and confirm repeated cycles settle within the budget.

### Publish complete text updates

Keep logical text and selection positions separate from the current rendered geometry. Publish the shaped layout and its glyph resources together when ready. Route any background shaping result through the owning UI thread using a generation or revision check.

**Check** rapidly change a label while resizing and closing its screen. Confirm stale work cannot replace newer text or target a reused widget, and that cursor and selection positions follow grapheme boundaries.
