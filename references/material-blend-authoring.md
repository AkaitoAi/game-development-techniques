# Material blend authoring

## Problem and design reasoning

**Problem.** Painted transitions look like blurred colors, dirt floats above cavities, and artists lose consistent surface response when mixing independently tuned materials.

**Developer reasoning.** For The Order: 1886, David Neubelt and Matt Pettineo described inheritance, material stacks, offline compositing, and runtime layering. Their stated production aim was efficient authoring with consistent material response across many assets. The SIGGRAPH 2013 notes and GDC 2014 talk document the pipeline during development of the subsequently shipped game. They support a reusable layer vocabulary, with the blend workflows below offered as engine-agnostic recommendations. [SIGGRAPH course notes](https://blog.selfshadow.com/publications/s2013-shading-course/rad/s2013_pbs_rad_notes.pdf) · [GDC talk](https://www.gdcvault.com/play/1020485/Crafting-a-Next-Gen-Material)

## Techniques and practical checks

### Describe the surface relationship

Label each layer as substrate, deposited coating, accumulated debris, exposed wear, or another authored role. Specify coverage, ordering, thickness cues, scale, and which properties the layer changes. A wetness effect may change roughness and color while retaining much of the underlying normal detail. Keep base materials reusable and store asset-specific coverage separately.

**Check** the layer stack in isolation with every layer enabled individually. Have another artist explain the physical relationship and reproduce the intended transition using the documented controls.

### Choose a blend rule with explicit limits

Use normalized weights for competing surface coverage and ordered masks for overlays. Define an all-zero fallback and overlap priority. Where height influences coverage, document height units, bias, and transition width. Height maps from unrelated sources need calibration before comparison. Keep a debug view showing final effective weights after all modifications.

**Check** zero weights, full overlap, equal heights, inverted height inputs, and extreme bias. Confirm weights remain finite and the surface has a predictable fallback without holes or unexpected layer dominance.

### Blend the complete response

Apply the selected coverage consistently to color, roughness, and other affected parameters. Decode color into linear light and blend normals using a method appropriate to their coordinate space and layer relationship. Validate normal length after combination. Establish a common world scale or explicitly document why a detail layer differs. Review transitions under moving illumination because similar base colors can conceal incompatible highlights.

**Check** the transition under neutral diffuse light, a small moving highlight, grazing light, and the intended game lighting. Look for seams, flat normal regions, abrupt roughness changes, and stretching on rotated assets.

### Preserve authorship through baking and LOD

Bake stable stacks where the production workflow and appearance permit it. Keep changing layers live and budget their texture samples and shader variants. Preserve the authored transition at distant mips and lower geometry detail. Store a reference capture and layer recipe with the asset. See [materials and textures](materials-and-textures.md) for import and residency decisions.

**Check** baked against live output, the lowest supported texture quality, compression, and a camera crossing LOD thresholds. Record appearance differences and frame cost in the busiest layered view before accepting the asset.
