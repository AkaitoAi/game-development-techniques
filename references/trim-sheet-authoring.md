# Trim sheet authoring

## Problem and design reasoning

Environment assets can repeat the same surface families while each new mesh still requires individual texture work. A trim sheet turns recurring strips, edges, and panels into an authoring contract between textures and geometry. The contract includes the layout, edge treatment, scale, and supported variation.

For shipped *Sunset Overdrive*, Insomniac needed a small environment team to create a large city. Morten Olsen's GDC presentation describes a standardized trim layout with normal-map bevels, allowing materials to be exchanged while preserving edge placement. The slides pair trim and plain tileable versions of each material. This supports a reusable surface workflow, with special-case geometry and hero details handled according to their visual needs. The layout checks below are portable recommendations. [Morten Olsen, The Ultimate Trim: Texturing Techniques of Sunset Overdrive, GDC 2015](https://www.gdcvault.com/play/1022324/The-Ultimate-Trim-Texturing-Techniques) · [Insomniac presentation slides](https://media.gdcvault.com/gdc2015/presentations/Olsen_Morten_TheUltimateTrim.pdf)

For compatible mesh dimensions and placement, see [Modular asset kit design](modular-asset-kit-design.md). For shader and import assumptions, see [Game material authoring validation](game-material-authoring-validation.md).

## Techniques and practical checks

### Design the layout from an asset family

Collect representative edge widths, panel proportions, and material families before laying out strips. Allocate space to shapes artists can reuse at the intended texel density. Name or document each strip's intended dimensions, orientation, and edge treatment. Include a plain surface option where a detailed strip would create unwanted repetition.

**Check** map a wall panel, narrow beam, broad frame, and curved or angled piece from the actual kit. Measure stretching and manual exceptions. Revise the layout when common pieces repeatedly require distorted UVs or unique materials.

### Make edge shading agree with geometry

Choose whether a visible edge needs geometry, an authored normal treatment, or both. Keep tangent conventions, UV orientation, and material interpretation consistent through export and import. A texture can suggest a bevel's lighting while the mesh still determines silhouette and collision.

**Check** inspect convex and concave corners under a moving grazing light from the gameplay camera. Include mirrored UVs and exported triangulation. Compare the lit result and silhouette and fix incorrect edge response before painting more detail.

### Protect filtering at the working distance

Give neighboring strips enough isolation for the project's mip and compression behavior. Establish where UVs may repeat and where they need a clean end. Keep normal, scalar, and color content aligned to the same layout contract.

**Check** capture close, middle, and distant views while moving past a repeated assembly. Inspect strip bleeding, shimmer, edge collapse, and seams in the lowest supported texture tier. Check the cooked textures in a player build.

### Validate reuse at the assembled-space level

Track how a shared sheet affects material count, memory, and authoring time. Keep material swaps aligned to the layout and use controlled variants where obvious repeated damage would draw attention. Validate new sheet revisions against existing consumers before publishing them.

**Check** ask a second artist to texture several kit pieces using the documented layout. Swap the supported material family and compare all consumers after a sheet revision. Record required UV corrections, memory use, draw cost, and repeated details visible during traversal.
