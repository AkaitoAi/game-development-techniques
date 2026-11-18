# Game material authoring validation

## Problem and design reasoning

A material may appear correct under its author's preview light while breaking under another exposure, texture mip, or platform variant. Validation needs a shared definition of what each channel means and representative rendering conditions. In *Call of Duty: WWII*, Sledgehammer changed gloss parameterization and used normal variation when generating gloss mips to reduce distant specular artifacts. The relationship between source maps and their runtime interpretation therefore matters as much as an individual texture's numeric range. [Danny Chan, Material Advances in Call of Duty: WWII, SIGGRAPH 2018](https://advances.realtimerendering.com/s2018/MaterialAdvancesInWWII-course_notes.pdf)

Sucker Punch describes *Ghost of Tsushima* materials as physically based with plausible authored values while allowing artistic lighting adjustments. This supports evaluating material response independently from final scene lighting. The validation workflow below is a portable recommendation derived from these production constraints. [Jasmin Patry, Real-Time Samurai Cinema, SIGGRAPH 2021](https://www.advances.realtimerendering.com/s2021/jpatry_advances2021/index.html)

## Techniques and practical checks

- Specify the material input contract. Record channel meaning, color space, normal orientation, roughness or gloss conversion, UV assumptions, and supported shader features. Validate import metadata against that contract. **Check** deliberately swap a linear scalar map to sRGB and invert the normal convention. Confirm the validator identifies the affected input and the reference scene makes its consequence visible.
- Maintain a small material comparison scene. Include neutral geometry, curved and flat surfaces, contrasting illumination, and a reproducible camera and exposure. Set acceptance criteria per material class. **Check** inspect representative dielectric, metallic, smooth, and rough samples under every reference condition. Compare with approved captures before judging a level's lighting.
- Validate filtering and scale together. Examine normal detail, roughness behavior, UV density, and compression across the viewing distances actually used in play. **Check** move the camera from close-up to the distant mip range and record motion. Inspect specular shimmer, detail collapse, edge seams, and whether the chosen fix respects the material's intended response.
- Validate the cooked variant. Track which features, texture formats, and material parameters survive each supported target and quality tier. **Check** build the lowest supported tier and a higher tier with the same fixture. Confirm required features render, scalar inputs remain in their intended units, and the material meets the measured pass and memory budgets.

See [Materials and textures](materials-and-textures.md) for import and sampling choices and [Runtime material parameter management](runtime-material-parameter-management.md) for gameplay changes to values.
