# Mesh vertex color workflows

## Problem and design reasoning

Vertex attributes let artists attach masks and variation to geometry without another texture, but their meaning can disappear between modeling, export, simplification, and shaders. In **Animating With Math**, GDC 2016, Natalie Burke explains shader-driven vertex animation in *Destiny*, including vertex color maps that refine animation. The useful reasoning is to let inexpensive mathematical motion gain spatial control through artist-authored data. [Natalie Burke, GDC 2016](https://www.gdcvault.com/play/1023249/Animating-With)

Bill Kladis's GDC 2018 *Fortnite* slides show vertex colors and Fresnel controlling husk eye emission within an automated material system. Ryan Brucks's GDC 2019 **Distance Fields and Simulation Tricks in UE4** also demonstrates encoding indices in vertex channels with an explicit quantization contract. These examples support vertex data as purposeful shader input; they do not imply that every mesh should use the same channel layout. The pipeline below is an engine-agnostic recommendation. [Kladis, GDC 2018 slides](https://media.gdcvault.com/gdc2018/presentations/Kladis_Autonomy_Real-time.pdf) · [Brucks, GDC 2019 slides](https://media.gdcvault.com/gdc2019/presentations/Brucks_Ryan_Distance_Fields_And.pdf)

## Techniques and practical checks

### Establish a typed channel contract

Document each channel's meaning, default, range, interpolation behavior, and owning shader family. Distinguish continuous masks from categorical IDs. Specify whether values are linear numeric data, not display colors, and whether alpha is retained. Brucks's eight-bit encoding example depends on increments of 1/255; other formats need their own precision rule.

**Check** export a diagnostic mesh containing zero, one, intermediate masks, and adjacent encoded IDs. Read back the runtime values and compare with the declared tolerance. Verify that import settings and color conversion do not alter data.

### Paint for actual topology and deformation

Masks interpolate across triangles, so a narrow effect cannot be controlled independently of vertex density. Plan topology around important boundaries, inspect triangulation, and understand split vertices at seams. For motion masks, consider the mesh's deformed pose and attachment area. Add geometry only where the control benefit justifies its rendering and skinning cost.

**Check** display each channel in isolation on the final triangulated mesh, then animate it through extreme poses. Examine bleeding across boundaries, seams, and fixed attachment regions; test at the intended camera distances.

### Preserve attributes through every derivative

Define how LOD generation, welding, mesh merging, instancing, and baking transfer attributes. Averaging a continuous blend mask may be acceptable; averaging categorical indices usually changes their meaning. Missing-channel defaults must be deliberate so an older mesh cannot silently enable maximum displacement or emission.

**Check** compare all shipping LODs and merged variants with a channel visualization. Force LOD switches while the effect is active, remove the channel from a test asset, and confirm approved fallback behavior rather than a visually plausible but incorrect result.

### Validate the rendered result and ownership

Keep an editable source of painted data and record which pipeline stage may modify it. Measure the cost of the chosen vertex format and any additional shader work. If runtime damage or customization also writes colors, allocate separate data or define composition explicitly so it does not overwrite authored animation masks.

**Check** combine every supported runtime modifier with the authored effect, reload the asset, and compare a reference capture. Inspect channel ownership and memory cost; confirm that runtime writes preserve unrelated masks and that the source asset reproduces the shipped result.

See [Shaders](shaders.md) for broader shading decisions and [3D modeling](3d-modeling.md) for geometry production.
