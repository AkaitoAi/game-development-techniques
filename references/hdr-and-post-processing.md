# HDR and post-processing

## Problem and design reasoning

**Problem.** A scene looks balanced on one display, but shadows disappear, highlights clip, or colors shift when it is viewed on a different output.

**Design reasoning.** Polyphony's work connects authoring, tone mapping, and output validation. Its goal of extending the SDR appearance into HDR points to preserving artistic intent while adapting the range to what the display can actually reproduce.

## Shipped examples

**Gran Turismo Sport (2017).** Polyphony Digital's SIGGRAPH Asia course describes how color science informed the full HDR workflow, from asset collection and preview to lighting and tone mapping. The delivery pipeline treated image appearance as a concern shared by artists and rendering engineers. [Polyphony Digital, SIGGRAPH Asia 2018 course notes](https://cdn2.gran-turismo.com/data/www/pdi_publications/siggraph_asia_2018_cousenotes.pdf)

**Gran Turismo 7 (2022).** Polyphony Digital presented a later tone-mapping approach for reproducing a wide range of luminance and color consistently across HDR and SDR outputs. The team describes HDR as an extension of the intended SDR appearance. [Polyphony Digital, SIGGRAPH 2025 course](https://s3.amazonaws.com/gran-turismo.com/pdi_publications/s2025_PBS_Physically_Based_Tone_Mapping_GT7.pdf)

## Techniques and practical checks

### Separate scene values from display output

Keep scene lighting and blending in a declared linear working space. Document input color transforms, including which textures contain data such as normals rather than color. Choose the display transform from the active output mode and avoid encoding the same transfer function twice.

**Check** a neutral ramp, saturated patches, and a bright emissive over dark geometry. Capture the intermediate working image and final output in SDR and HDR. Inspect smooth gradation, intended gray values, and bright color without unexpected clipping or hue shifts.

### Make exposure adaptation predictable

Set a bounded exposure range and choose how the scene is metered. Tune adaptation separately for entering bright and dark locations. Add explicit controls for authored transitions when automated adaptation obscures an important cue.

**Check** a repeatable walk from a dark room into direct light and back. Compare adaptation while looking at a bright window, toward a dark floor, and after a camera cut. Verify that interaction cues remain visible without flickering between exposure levels.

### Keep interfaces readable over the scene

Set a deliberate reference luminance for interface elements in HDR and choose where they are composited relative to exposure, bloom, and tone mapping. Reserve strong bloom, grain, and vignettes for intended use, and separate their controls when they affect readability or comfort.

**Check** subtitles and HUD markings over black, midtone, and bright scenes. Test on both SDR and HDR targets, including a lower peak-brightness HDR display. Toggle each configurable effect and inspect contrast and perceived interface brightness.

### Test display changes and post-processing cost

Update output mode and calibration when presentation capabilities change. Rebuild mode-dependent resources and invalidate stale history when required. Measure the cost of full-screen passes by resolution and preset and lower the resolution of effects where it preserves the desired look.

**Check** runtime display-mode switches, HDR fallback, resolution changes, and capture or streaming paths the project supports. Verify that the presented frame and captured image follow their intended output transforms. Record the GPU cost of each pass at the highest supported resolution.
